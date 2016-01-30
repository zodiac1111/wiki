# 使用netcat调试以太网程序

[natcat](http://zh.wikipedia.org/wiki/Netcat)自定义发送接受数据,调试以太网程序.

* 参考例子: http://www.rationallyparanoid.com/articles/netcat.html

实现:
* `nc`/`netcat` 传统的netcat
* `socat` netcat 较复杂的姊妹程式,更大更复杂,更多的选项.
* `ncat` 由[Nmap](http://zh.wikipedia.org/wiki/Nmap)开发团队实做的另一个netcat版本。
* [Cryptcat](http://sourceforge.net/projects/cryptcat/) 是 netcat 一个内建加密传输能力的版本。

# nc

## 安装

## 客户端

通过udp模式发送特定数据到指定主机的指定端口

```
echo -ne "123456\r\n" | nc -u 127.0.0.1 8001
```
* `nc` natcat 网络实用工具
 * `-u` udp模式,不加则是tcp模式
 * `127.0.0.1` 主机
 * `8001` 端口

也可以直接`nc 127.0.0.1 8002`然后从控制台输入,回车后发送.

## 服务端

```
nc -l -p 8002 -v  -k
```


# ncat

## 安装

todo

## 客户端

## 服务器


```
ncat -l -p 8002 -vv  -k
```
* `-l` listen 监听
* `-p 8002` port指定端口,ncat无需-p,
* `-vv` 显示详细信息
* `-k` keep保持连接,ncat实现.nc不可


# netcat

## 安装

debian
```
apt-get install netcat
```
# 客户端



# 服务器

```
netcat -l -p 8002 -vv  -k
```
