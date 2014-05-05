# bbb debian wifi 配置

参考:http://my.oschina.net/robeer/blog/207715

## 查看硬件(usb网卡)
```bash
sudo lsusb
```

## 安装无线上网工具
```bash
sudo aptitude install wireless-tools
```

## 查询无线网卡是否正常
```bash
sudo ifconfig -a
```
## 启用你的wlan0节点
```bash
sudo ifconfig wlan0 up
```

## 扫描你的无线网络
```bash
sudo iwlist wlan0 scan
```
## 生成密钥文件 
```bash
sudo wpa_passphrase  TEST  12345678  > /etc/wpa_supplicant.conf 
sudo wpa_passphrase  <ssid>  <密码>  > /etc/wpa_supplicant.conf 
```

## 连接你的无线路由器 
注意：有使用ifup之前，请先检查你的配置文件。否则会报错的，这个是特别需要注意的。也就是要更改/etc/network/interfaces文件。
```bash
vim /etc/network/interfaces
```

例子:
```bash
######  无线  ########
# WiFi Example
auto wlan0

# 动态获取(二选一)
iface wlan0 inet dhcp
    wpa-ssid "<SSID>"
    wpa-psk  "<密码>"

# 静态分配(二选一)
#iface wlan0 inet static
#    address 192.168.1.203
#    netmask 255.255.255.0
#    network 192.168.1.0
#    gateway 192.168.1.1
```
## 启动wlan0
```bash
root@arm:~# ifup wlan0
Internet Systems Consortium DHCP Client 4.2.4
Copyright 2004-2012 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/wlan0/xx:xx:6d:xx:xx:e8
Sending on   LPF/wlan0/xx:xx:6d:xx:xx:e8
Sending on   Socket/fallback
DHCPREQUEST on wlan0 to 255.255.255.255 port 67
DHCPREQUEST on wlan0 to 255.255.255.255 port 67
DHCPREQUEST on wlan0 to 255.255.255.255 port 67
DHCPDISCOVER on wlan0 to 255.255.255.255 port 67 interval 3
DHCPDISCOVER on wlan0 to 255.255.255.255 port 67 interval 4
DHCPDISCOVER on wlan0 to 255.255.255.255 port 67 interval 11
DHCPDISCOVER on wlan0 to 255.255.255.255 port 67 interval 20
DHCPREQUEST on wlan0 to 255.255.255.255 port 67
DHCPOFFER from 192.168.1.1
DHCPACK from 192.168.1.1
bound to 192.168.1.136 -- renewal in 39170 seconds.
```

## 测试一下网络
```bash
ping 8.8.8.8
ping baidu.com
```