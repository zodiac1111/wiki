# 各种方式的串口监视

# strace

```bash
strace -s9999 -o myapp.strace -eread,write,ioctl ./myapp
```

返回类似
```text
write(3, ",dns=", 5)                    = 5
write(3, "192.168.2.1", 11)             = 11
write(3, "\r\n", 2)                     = 2
ioctl(4, SNDCTL_TMR_TIMEBASE or TCGETS, 0xbed13acc) = -1 ENOTTY (Inappropriate ioctl for device)
read(4, "[1]\nprotocol=15\nport=1\ntype=uart\nname=com1\nenable=1\n\n[2]\nprotocol=15\nport=2\ntype=uart\nname=com2\nenable=1\n\n[3]\nprotocol=15\nport=3\ntype=uart\nname=com3\nenable=0\n\n[4]\nprotocol=15\nport=4\ntype=uart\nname=com4\nenable=0\n\n[5]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[6]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[7]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[8]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[9]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[10]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[11]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[12]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[13]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[14]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[15]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n\n[16]\nprotocol=0\nport=-\ntype=uart\nname=-\nenable=0\n", 4096) = 806
read(4, "", 4096)                       = 0
read(4, "UTC-8\n", 68)                  = 6
```


优点

* 不烦扰原来程序的运行,透明监视
* 保存文件

缺点

* 把所有read/write都追踪了,比如网路等,比较杂/多
* c整合可能不太方便
* 不能不影响运行的情况下启动/关闭监视
* 输出字符串

# 调试串口程序

When I debug interaction of my application with a serial port, I use [moserial](https://help.gnome.org/users/moserial/stable/basic-usage.html.en).

# usb串口检视

http://sourceforge.net/projects/ttyusbspy/?source=navbar 仅限usb

wireshark 也可以监视usb

# socat

[使用socat实现Linux虚拟串口](http://blog.csdn.net/rainertop/article/details/26706847)

（1）打开终端0，输入
```
socat -d -d pty,raw,echo=0 pty,raw,echo=0
```
返回结果：
```
2014/05/23 14:56:05 socat[2220] N PTY is /dev/pts/1
2014/05/23 14:56:05 socat[2220] N PTY is /dev/pts/3
2014/05/23 14:56:05 socat[2220] N starting data transfer loop with FDs [3,3] and [5,5]
```
（2）打开终端1，输入
```
cat < /dev/pst/1
```
（3）打开另一个终端2，输入
```
echo "Test" > /dev/pts/3，此时，终端1中会显示“Test”。
```
上述实验在vmware + ubuntu 12.04 环境下完成。

原文地址：http://stackoverflow.com/questions/52187/virtual-serial-port-for-linux