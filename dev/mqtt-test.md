=====

使用`mosquitto-1.4.2`测试mqtt协议.

角色定义

* ser mqtt服务器(公网)
	* `Linux 1 2.6.32-042stab092.1 #1 SMP Tue Jun 24 09:10:28 MSK 2014 x86_64 GNU/Linux`
* sub 订阅者终端设备(内网)
	* `Linux AT91SAM9-RT9x5 2.6.39 #34 Wed Jun 4 16:12:41 CST 2014 armv5tejl GNU/Linux`
* pub 发布者(内外网均可,测试用内网)
	* `Linux debian 3.16.0-4-amd64 #1 SMP Debian 3.16.7-ckt11-1 (2015-05-24) x86_64 GNU/Linux`

# 服务器

服务器安装运行mosquitto server

	apt-get install mosquitto

# 订阅者

订阅者订阅topic

	./mosquitto_sub -h ser -t cmd -v
	# -h ser 指定mqtt服务器:ser
	# -t cmd 指定topic(主题):cmd
	# -v 打印更多调试信息

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

订阅者列出当前目录内容

#







