# 非root用户使用wireshark

来源: 
* http://blog.bravi.org/?p=912
* http://crunchbang.org/forums/viewtopic.php?id=18182

1. 可能需要加载usb监视模块
```
sudo modprobe usbmon
```

2. Install setcap tool in Ubuntu, simply use APT:
```
sudo apt-get install libcap2-bin
```

3. Create a Wireshark Group and assign current user to that group:
```
sudo addgroup -quiet -system wireshark
usermod -a -G wireshark stretch
```
4. Gather group rights for current user:
```
newgrp wireshark
```
5. Assign the dumpcap executable to this group, as dumpcap is responsible for all the low-level capture work. Changing its mode to 750 ensures only users belonging to its group can execute the file:
```
sudo chown root:wireshark /usr/bin/dumpcap
chmod 750 /usr/bin/dumpcap
```
6. Grant capabilities with setcap:设置能力
```
sudo setcap\
 cap_net_raw,cap_net_admin,cap_dac_override+eip\
 /usr/bin/dumpcap
```
7. Verify set capabilities (as unprivileged user):验证设置能力
```
getcap /usr/bin/dumpcap
```
显示
```
/usr/bin/dumpcap = cap_dac_override,cap_net_admin,cap_net_raw+eip
```
8. List available network interfaces (as unprivileged user):验证
```
dumpcap -D
1. eth0
2. wlan0
3. usbmon1 (USB bus number 1)
4. usbmon2 (USB bus number 2)
5. usbmon3 (USB bus number 3)
6. usbmon4 (USB bus number 4)
7. any (Pseudo-device that captures on all interfaces)
8. lo
```

# 采集usb

上面的

1.可能需要加载usb监视模块
```
sudo modprobe usbmon
```
```
sudo modprobe usbmon
```
和

6.Grant capabilities with setcap:设置能力
```
sudo setcap\
 cap_net_raw,cap_net_admin,cap_dac_override+eip\
 /usr/bin/dumpcap
```

用这个可以检测

8.List available network interfaces (as unprivileged user):验证
```
dumpcap -D
1. eth0
2. wlan0
3. usbmon1 (USB bus number 1)
4. usbmon2 (USB bus number 2)
5. usbmon3 (USB bus number 3)
6. usbmon4 (USB bus number 4)
7. any (Pseudo-device that captures on all interfaces)
8. lo