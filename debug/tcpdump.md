# tcpdump

简明入门参考

> * [美观的比较全的](http://www.danielmiessler.com/study/tcpdump/)
> * [简单各种使用英文](http://www.rationallyparanoid.com/articles/tcpdump.html)
> * [国内的共享中文evernote](https://www.evernote.com/shard/s121/sh/45a809d9-aa4a-4e47-a569-7728a269f4b1/2cac4a3af64b952b232df9df25e8fee4)

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

# 参考开源的备份

```text
In most cases you will need root permission to be able to capture packets on an interface. Using tcpdump (with root) to capture the packets and saving them to a file to analyze with Wireshark (using a regular account) is recommended over using Wireshark with a root account to capture packets on an "untrusted" interface. See the Wireshark security advisories for reasons why.

See the list of interfaces on which tcpdump can listen:
tcpdump -D

Listen on interface eth0:
tcpdump -i eth0

Listen on any available interface (cannot be done in promiscuous mode. Requires Linux kernel 2.2 or greater):
tcpdump -i any

Be verbose while capturing packets:
tcpdump -v

Be more verbose while capturing packets:
tcpdump -vv

Be very verbose while capturing packets:
tcpdump -vvv

Be less verbose (than the default) while capturing packets:
tcpdump -q

Limit the capture to 100 packets:
tcpdump -c 100

Record the packet capture to a file called capture.cap:
tcpdump -w capture.cap

Record the packet capture to a file called capture.cap but display on-screen how many packets have been captured in real-time:
tcpdump -v -w capture.cap

Display the packets of a file called capture.cap:
tcpdump -r capture.cap

Display the packets using maximum detail of a file called capture.cap:
tcpdump -vvv -r capture.cap

Display IP addresses and port numbers instead of domain and service names when capturing packets:
tcpdump -n

Capture any packets where the destination host is 192.168.1.1. Display IP addresses and port numbers:
tcpdump -n dst host 192.168.1.1

Capture any packets where the source host is 192.168.1.1. Display IP addresses and port numbers:
tcpdump -n src host 192.168.1.1

Capture any packets where the source or destination host is 192.168.1.1. Display IP addresses and port numbers:
tcpdump -n host 192.168.1.1

Capture any packets where the destination network is 192.168.1.0/24. Display IP addresses and port numbers:
tcpdump -n dst net 192.168.1.0/24

Capture any packets where the source network is 192.168.1.0/24. Display IP addresses and port numbers:
tcpdump -n src net 192.168.1.0/24

Capture any packets where the source or destination network is 192.168.1.0/24. Display IP addresses and port numbers:
tcpdump -n net 192.168.1.0/24

Capture any packets where the destination port is 23. Display IP addresses and port numbers:
tcpdump -n dst port 23

Capture any packets where the destination port is is between 1 and 1023 inclusive. Display IP addresses and port numbers:
tcpdump -n dst portrange 1-1023

Capture only TCP packets where the destination port is is between 1 and 1023 inclusive. Display IP addresses and port numbers:
tcpdump -n tcp dst portrange 1-1023

Capture only UDP packets where the destination port is is between 1 and 1023 inclusive. Display IP addresses and port numbers:
tcpdump -n udp dst portrange 1-1023

Capture any packets with destination IP 192.168.1.1 and destination port 23. Display IP addresses and port numbers:
tcpdump -n "dst host 192.168.1.1 and dst port 23"

Capture any packets with destination IP 192.168.1.1 and destination port 80 or 443. Display IP addresses and port numbers:
tcpdump -n "dst host 192.168.1.1 and (dst port 80 or dst port 443)"

Capture any ICMP packets:
tcpdump -v icmp

Capture any ARP packets:
tcpdump -v arp

Capture either ICMP or ARP packets:
tcpdump -v "icmp or arp"

Capture any packets that are broadcast or multicast:
tcpdump -n "broadcast or multicast"

Capture 500 bytes of data for each packet rather than the default of 68 bytes:
tcpdump -s 500

Capture all bytes of data within the packet:
tcpdump -s 0
```
