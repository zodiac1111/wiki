# bbb debian wifi 配置

参考:http://my.oschina.net/robeer/blog/207715

查看硬件(usb网卡)
```bash
sudo lsusb
```

安装无线上网工具
```bash
sudo aptitude install wireless-tools
```

查询无线网卡是否正常
```bash
sudo ifconfig -a
```
启用你的wlan0节点
```bash
sudo ifconfig wlan0 up
```

扫描你的无线网络
```bash
sudo iwlist wlan0 scan
```
生成密钥文件 
```bash
sudo wpa_passphrase  TEST  12345678  > /etc/wpa_supplicant.conf 
sudo wpa_passphrase  <ssid>  <密码>  > /etc/wpa_supplicant.conf 
```