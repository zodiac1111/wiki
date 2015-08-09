# mosquitto

物联网传输协议,ibm开发

[[http://www.hivemq.com/wp-content/uploads/mqtt-tcp-ip-stack.png]]

网络连接示意图

[[http://www.hivemq.com/wp-content/uploads/connect-flow.png]]

包例子,很多都是可选的

[[http://www.hivemq.com/wp-content/uploads/connect.png]]

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


# 参考

* http://www.hivemq.com/mqtt-essentials-part-3-client-broker-connection-establishment/

# 参考,中文

* [MQTT协议笔记之连接和心跳](https://www.google.com.hk/url?sa=t&rct=j&q=&esrc=s&source=web&cd=14&ved=0CDEQFjADOApqFQoTCOHJicuvnMcCFYTZLAodErQATg&url=http%3a%2f%2fwww%2eblogjava%2enet%2fyongboy%2farchive%2f2015%2f04%2f23%2f409630%2ehtml&ei=N3nHVaHSK4SzswGS6ILwBA&usg=AFQjCNHBeglW__iEpsTfpZZLp2_6A9pmjg)