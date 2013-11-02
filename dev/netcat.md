# 使用netcat调试以太网程序

[natcat](http://zh.wikipedia.org/wiki/Netcat)自定义发送接受数据,调试以太网程序.

* 参考例子: http://www.rationallyparanoid.com/articles/netcat.html

实现:
* `nc`/`netcat` 传统的netcat
* `socat` netcat 较复杂的姊妹程式。它比起netcat更大更复杂，并且有更多的选项得在给定作业前先设定。
* `ncat` 由[Nmap](http://zh.wikipedia.org/wiki/Nmap)开发团队实做的另一个netcat版本。
* [Cryptcat](http://sourceforge.net/projects/cryptcat/) 是 netcat 一个内建加密传输能力的版本。

# 例子(作为客户端)

通过udp模式发送特定数据到指定主机的指定端口

```
echo -ne "123456\r\n" | nc -u 127.0.0.1 8001
```

* `echo`构造输入数据
 * -n 不要附加换行
 * -e 翻译转义字符
 * "123456\r\n" 待发送数据
* nc `natcat`网络实用工具
 * -u udp模式
 * 127.0.0.1 主机
 * 8001 端口

# 例子(作为服务器)

```
nc -l -p -vv 8002
```
* -l listen 监听
* -p 8000 port指定端口
* -vv 显示详细信息