# mosquitto

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
6) 发:ping 请求
7) 收:ping 响应 (定时重复)
```