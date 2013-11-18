# tcpdump

# 简单使用

###### 简单16进制查看
```bash
tcpdump -XX -i wlan
```
* `-XX` 以16进制显示
* `-i wlan0`interface为wlan0,可以`ifconfig`自行查看

##### 保存显示数据到文件,可供wireshark等软件打开,做进一步分析
```bash
 tcpdump -w save.pcap -i eth0
```