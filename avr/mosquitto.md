# mosquitto

测试服务器 http://test.mosquitto.org/

简单测试

    mosquitto_sub -h test.mosquitto.org -t "#" -v

默认端口 1883

    mosquitto_sub -h test.mosquitto.org -t "hello" 

参数

  -h 主机名
  -t topic 主题
