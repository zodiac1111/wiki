# tcpdump

简明入门参考

> * http://www.danielmiessler.com/study/tcpdump/
> * 国内的共享 https://www.evernote.com/shard/s121/sh/45a809d9-aa4a-4e47-a569-7728a269f4b1/2cac4a3af64b952b232df9df25e8fee4

部分高级的技巧

> * http://docstore.mik.ua/orelly/networking_2ndEd/tshoot/index.htm

# 简单使用

##### 16进制查看
```bash
tcpdump -XX -i wlan
```
* `-XX` 以16进制显示
* `-i wlan0`interface为wlan0,可以`ifconfig`自行查看

##### 保存到文件
保存显示数据到文件,可供wireshark等软件打开,做进一步分析
```bash
 tcpdump -w save.pcap -i eth0
```
* -w write,写原始数据到文件
* -i 指定interface

##### 指定端口
指定端口,例如抓取web数据,80端口
```bash
tcpdump -A -i wlan0 port 80
```
* -A ascii 以ascii码显示,http是基于可见的字符的
* port 80 指定抓取80端口的数据.

# 指定协议

    tcpdump tcp
    tcpdump udp

# webbox监视gprs报文例子

```bash
tcpdump -w save.pcap -i ppp0 port 80 -vv
```

# 详细抓包

```bash
tcpdump -AXX  -vv  -i ppp0 port 80 
```

* 结合A ascii码和 XX 十六进制显示
* -vv 非常详细模式
* 如果要保存,再使用-w <文件名. 保存
* -i 指定interface
* port 指定端口
