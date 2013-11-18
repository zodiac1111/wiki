# tcpdump

# 简单使用

##### 简单16进制查看
```bash
tcpdump -XX -i wlan
```
* `-XX` 以16进制显示
* `-i wlan0`interface为wlan0,可以`ifconfig`自行查看

##### 保存显示数据到文件,可供wireshark等软件打开,做进一步分析
```bash
 tcpdump -w save.pcap -i eth0
```
* -w write,写原始数据到文件
* -i 指定interface

##### 指定端口,例如抓取web数据
```bash
tcpdump -A -i wlan0 port 80
```
* -A ascii 以ascii码显示,http是基于可见的字符的
* port 80 指定抓取80端口的数据.