# mqtt协议

同类协议[coap](/dev/coap)

物联网传输协议,ibm开发

[[http://www.hivemq.com/wp-content/uploads/mqtt-tcp-ip-stack.png]]

网络连接示意图

[[http://www.hivemq.com/wp-content/uploads/connect-flow.png]]

包例子,很多都是可选的

[[http://www.hivemq.com/wp-content/uploads/connect.png]]

# 简单例子

* subscribe 订阅 抓取数据
* publish 发布 发送数据

测试服务器 http://test.mosquitto.org/

简单测试

    mosquitto_sub -h test.mosquitto.org -t "#" -v

默认端口 1883

    mosquitto_sub -h test.mosquitto.org -t "hello" 

参数

    -h 主机名
    -t topic 主题

简单流程

```
1. 发:连接命令
2. 收:ack
3. 发:订阅请求 topic:hello
4. 收:ack
5. 收:Publish message 发布的消息 "ggjhkgplopplopjfjkljdqsjklmjspppkmd23502"
6) 发:ping 请求 (定时重复)
7) 收:ping 响应 (定时重复)
```
# mosquitto

## 编译

首先设置`config.mk`中所有选项为no

    make  # host编译
    make CC=arm-linux-gcc CXX=arm-linux-gcc #交叉编译

## 测试

    ./mosquitto_sub -h test.mosquitto.org -t "hello"

    
# 既有实现
## 客户端

* [mosquitto](http://mosquitto.org/)
* [java实现的客户端](https://github.com/fusesource/mqtt-client)
* [chrome插件](https://chrome.google.com/webstore/detail/mqttlens/hemojaaeigabkbcookmlgmdigohjobjm)

## 服务端

* ibm的,不开源,需注册(放弃)
* 比较少

# 参考

## 英文
* [官网](http://mqtt.org/)
* [文档](http://mqtt.org/documentation),[pdf](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.pdf)
* [维基百科](https://en.wikipedia.org/wiki/MQTT),[日本語](https://en.wikipedia.org/wiki/MQTT)
* http://www.hivemq.com/mqtt-essentials-part-3-client-broker-connection-establishment/

## 中文

* [通过 WebSphere MQ 遥测传输 (MQTT) 将 Android 手机引入物联网ibm](https://www.ibm.com/developerworks/cn/websphere/library/techarticles/1109_wangb_mqandroid/1109_wangb_mqandroid.html)
  * 安卓相关,写的一般
* [MQTT协议笔记之连接和心跳](https://www.google.com.hk/url?sa=t&rct=j&q=&esrc=s&source=web&cd=14&ved=0CDEQFjADOApqFQoTCOHJicuvnMcCFYTZLAodErQATg&url=http%3a%2f%2fwww%2eblogjava%2enet%2fyongboy%2farchive%2f2015%2f04%2f23%2f409630%2ehtml&ei=N3nHVaHSK4SzswGS6ILwBA&usg=AFQjCNHBeglW__iEpsTfpZZLp2_6A9pmjg)
  * 报文结构分析很详细,推荐
* [物联网协议比较 MQTT CoAP RESTful/HTTP XMPP](http://www.phodal.com/blog/iot-protocols-coap-mqtt-xmpp-restful-http/)
  * 一句话接收,没啥比较(鸡肋):CoAP唯一可比的协议:[wiki](https://en.wikipedia.org/wiki/Constrained_Application_Protocol),

