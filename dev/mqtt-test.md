测试mqtt
=====

使用`mosquitto-1.4.2`测试mqtt协议.

角色定义

* `ser` 服务器broker,mqtt服务器/代理(公网)
	* `Linux 1 2.6.32-042stab092.1 #1 SMP Tue Jun 24 09:10:28 MSK 2014 x86_64 GNU/Linux`
* `sub` 客户端client,subscriber,订阅者终端设备(内网)
	* `Linux AT91SAM9-RT9x5 2.6.39 #34 Wed Jun 4 16:12:41 CST 2014 armv5tejl GNU/Linux`
* `pub` 客户端client,publisher,发布者(内外网均可,本例使用内网)
	* `Linux debian 3.16.0-4-amd64 #1 SMP Debian 3.16.7-ckt11-1 (2015-05-24) x86_64 GNU/Linux`

# 服务器

服务器安装运行mosquitto服务.

	apt-get install mosquitto

# 订阅者

订阅者订阅topic

	./mosquitto_sub -v -d -h ser -t cmd
	# -v 打印更多调试信息
	# -d 启动调试信息
	# -h ser 指定mqtt服务器:ser
	# -t cmd 指定topic(主题):cmd


订阅者实现接受cmd topic后执行message的指令.实现简单的shell程序.

# 发布者

发布者发布topic

	./mosquitto_pub_x64 -h ser -d -t cmd -m "ls"
	# -h ser 指定mqtt服务器:ser
	# -d 启动调试信息
	# -t cmd 指定topic(主题):cmd
	# -m "ls" 指定message:ls

# 运行

发布后运行后类似如下结果

	user@debian:client$ ./mosquitto_pub_x64 -h ser -d -t cmd -m "ls"
	Client mosqpub/6249-debian sending CONNECT
	Client mosqpub/6249-debian received CONNACK
	Client mosqpub/6249-debian sending PUBLISH (d0, q0, r0, m1, 'cmd', ... (2 bytes))
	Client mosqpub/6249-debian sending DISCONNECT

订阅者类似如下内容

	[root@AT91SAM9-RT9x5 /data]# ./mosquitto_sub -h ser -v -d -t cmd
	Client mosqsub/972-AT91SAM9-RT sending CONNECT
	Client mosqsub/972-AT91SAM9-RT received CONNACK
	Client mosqsub/972-AT91SAM9-RT sending SUBSCRIBE (Mid: 1, Topic: demo, QoS: 0)
	Client mosqsub/972-AT91SAM9-RT received SUBACK
	Subscribed (mid: 1): 0

发布时订阅者打印

	Client mosqsub/972-AT91SAM9-RT received PUBLISH (d0, q0, r0, m0, 'ls', ... (2 bytes))

订阅者keepalive

	Client mosqsub/972-AT91SAM9-RT sending PINGREQ
	Client mosqsub/972-AT91SAM9-RT received PINGRESP

# 发布类型

发送空消息(null)

调试时比较好用

	./mosquitto_pub_x64 -h ser -d -t demo -n

send message

	./mosquitto_pub_x64 -h ser -d -t demo -m "hello world"

send file

	./mosquitto_pub_x64 -h ser -d -t demo -f ./test_file.txt

read from stdin(例如终端),然后发送

	echo "hello world" | ./mosquitto_pub_x64 -h ser -d -t demo -s

遗嘱

	./mosquitto_pub_x64 -h ser -d -t demo -m "hello world" \
		--will-topic helpme --will-payload "`date '+%Y/%m/%d %T'`"

# topic命名

命名参考:[mqtt topics best practices](http://www.hivemq.com/mqtt-essentials-part-5-mqtt-topics-best-practices/)

* ascii 字符,大小写敏感
* `/` 目录分割符
* `+` 通配符一级目录
* `#` 通配符多级目录

一些topic例子

	myhome/groundfloor/livingroom/temperature
	USA/California/San Francisco/Silicon Valley
	5ff4a2ce-e485-40f4-826c-b1a5d81be9b6/status
	Germany/Bavaria/car/2382340923453/latitude

实现

	# path定义
	prj/gw/ch/dev/v
	# prj project 工程
	# gw  gateway 网关
	# ch  channel 通道
	# dev device  设备
	# v   value   变量

建议

* 不要使用先导的斜杠'/'
* 不要使用空格
* 简明扼要
* 使用ascii码,不要使用不可打印的字符
* 不要定订阅`#`
* 内嵌一个唯一标识符或者客户端id

# QoS

参考[MQTT Essentials Part 6: Quality of Service 0, 1 & 2](http://www.hivemq.com/mqtt-essentials-part-6-mqtt-quality-of-service-levels/).

客户端到代理的流量策略

根据数据的重要性,连接的重要性自行判断

* 越简单越省流量
* 越精确代价越高
* 至多一次最省流量
* 有且只有一次最耗流量

具体应用场景详见以上参考链接.

