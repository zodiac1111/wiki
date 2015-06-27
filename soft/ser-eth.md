# socat使用

# 语法

	socat [选项] <端口1> <端口2>

以以太网不同配置分类:

* tcp
  * tcp client 模式
  * tcp server 模式
* udp


# 解析

tcp client 模式

	socat -d -d  file:/dev/ttyUSB0, tcp:192.168.1.114:1027
	socat -x -d -d -d -d tcp:192.168.1.114:1031 file:/dev/ttyS1,b9600,echo=0,raw,nonblock,vmin=10,vtime=255

解释


* -x 以16进制显示发送和接收的内容.
* -d 0~4个调试等级,输出调试信息.
* 端口1(以太网)参数:
  * tcp:tcp服务
  * 192.168.1.114 IP
  * 1031 端口
* 端口2(串口)参数:
  * file 类型,文件(设备文件)
  * /dev/ttyS1 串口设备文件
  * b9600 波特率
  * echo=0 关闭回显
  * raw 原始格式,不转义\r\n
  * nonblock 不阻塞
  * 串口超时属性,两者或关系.接收大于255字节或者字节间超过1秒没数据就是一帧.
    * vmin=10 10*100ms没有数据则超时结束,认为是一帧.
    * vtime=255 串口接收大于255个字节则认为是一帧.

tcp server 模式

	socat -d -d  file:/dev/ttyUSB0,nonblock,raw,crnl tcp-l:2000,reuseaddr,fork
	socat -x -d -d -d -d tcp-l:2000,reuseaddr,fork file:/dev/ttyS1,b9600,echo=0,raw,nonblock,vmin=10,vtime=255

* -x (和上面一样)
* -d (和上面一样)
* 端口1(以太网)参数:
  * tcp-l tcp listen监听,即作为服务器
  * 2000 监听绑定的端口
  * reuseaddr 重使用ip地址
  * fork 创建新的进程用于接收客户端连接
* 端口2(串口)参数:(和上面一样)



# 例子

M COM - TCPS:port == TCPC - COM S

socat  pty,link=/dev/virtualcom0,raw  tcp:192.168.254.254:8080&


M COM - TCPC == TCPS:port - COM S

socat TCP-LISTEN:2000,fork /dev/ttyUSB0,raw
sudo socat -d tcp-l:2000,reuseaddr,fork  file:/dev/ttyUSB0,nonblock,waitlock=/var/run/ttyUSB0.lock
