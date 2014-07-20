# BeagleBone Black 相关笔记

ARM EABIhf

hf=hardware float 硬件浮点型

# 准备
##  硬件获得
* 官网 http://beagleboard.org/black
* 购买: chipsee 官方指定中国代理商;  http://www.chipsee.com/  
  * 淘宝 http://item.taobao.com/item.htm?id=20044299382
* 中国版BB-Black 英蓓特 http://www.embedinfo.com/ (我没有使用这个版本)

最新的镜像文件可以在[这里](http://beagleboard.org/latest-images/)获得,包括debian和Angstrom
# 资料

* elinux的维基 http://elinux.org/Beagleboard:BeagleBoneBlack

# 安装debian

* 1 [简单博客](http://blogs.bu.edu/mhirsch/2013/11/install-debian-7-to-emmc-internal-flash-drive-of-beaglebone-black/) 安装到eMMC ,他的文件是[这里](https://rcn-ee.net/deb/microsd/wheezy/) 下的,下bone * img.xz ,[flasher](https://rcn-ee.net/deb/flasher/wheezy/)
* 1.1 [安装debian/ubuntu](http://www.gigamegablog.com/2012/09/03/ubuntu-on-the-beaglebone-enabling-analog-in-pwm-i2c-and-spi/)
* 2 [adafruit的专题](http://learn.adafruit.com/downloads/pdf/beaglebone-black-installing-operating-systems.pdf)
* 3 [elinux.org bbb安装debian](http://elinux.org/Beagleboard:Debian_On_BeagleBone_Black)
* 4 [比较全面,从bootloader,交叉编译器到根文件系统都有](http://eewiki.net/display/linuxonarm/BeagleBone+Black)


A: 使用sd卡引导,不安装到flash

## 我的尝试(成功)

主要参考上面1

B: 安装到flash (我的选择)

主要步骤参考1, 镜像在1的目录上找到

* 复制到sd卡
* 把sd卡插到bbb上,While holding down the 'boot' button, apply power to the board. Continue to hold the 'boot' button until the USER LEDs begin to flash 用户名密码 debian/temppwd
* 启动sd卡上的系统(类似livecd)
* 从host复制镜像到bbb上 (这个livecd内容小,所有从host安装,类似livecd安装到硬盘)
* 在bbb上,解压镜像到块设备上 lsblk ` mmcblk1      179:8    0   1.8G  0 disk` 类似镜像恢复

### 再来一次的记录(启动livecd系统)

参考

> http://blogs.bu.edu/mhirsch/2013/11/install-debian-7-to-emmc-internal-flash-drive-of-beaglebone-black/
> http://elinux.org/Beagleboard:Debian_On_BeagleBone_Black

主要步骤

* 从[这里](http://elinux.org/Beagleboard:Debian_On_BeagleBone_Black)看到
* 下载 img.xz 文件而不是 .tar.xz文件,比如我下载`bone-debian-7.5-2014-05-06-2gb.img.xz`
* 解压这个文件,官方说用7z,我用debian自带的也行.
* 得到文件:`bone-debian-7.5-2014-05-06-2gb.img`
* window磁盘写入工具,我用debian自带的"磁盘"工具
* 电脑上插入microSD卡,我用usb读卡器也是可以的
* host:附件>磁盘找到卡的那个磁盘.设置按钮>从磁盘映像恢复>选择`bone-debian-7.5-2014-05-06-2gb.img`文件>开始恢复
* 等
* 完成后,bbb关机,插到bbb上
* 如果可能,连接串口,连接电源适配器.
* 按住boot按钮.开机
* 直到 USER LED 开始闪烁 (一会儿的事)
* 约一分钟后可以看到命令提示符,登陆

```bash
systemd-fsck[218]: rootfs: clean, 23493/102752 files, 115814/410368 blocks              
[   10.042624] libphy: PHY 4a101000.mdio:01 not found                                   
[   10.047677] net eth0: phy 4a101000.mdio:01 not found on slave 1                      
# 这里等了比较久 = =                                                                                   
Debian GNU/Linux 7 arm ttyO0                                                            
	                                                                                    
default username:password is [debian:temppwd]                                           
	                                                                                    
The IP Address for usb0 is: 192.168.7.2                                                 
arm login: debian                                                                       
Password:                                                                               
Linux arm 3.8.13-bone49 #1 SMP Fri May 2 06:36:13 UTC 2014 armv7l                       
	                                                                                    
The programs included with the Debian GNU/Linux system are free software;               
the exact distribution terms for each program are described in the                      
individual files in /usr/share/doc/*/copyright.                                         
	                                                                                    
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent                       
permitted by applicable law.                                                            
debian@arm:~$ 
```

到目前为止类似livecd的模式已经可以了.接下来就是通过这个livecd模式把文件下载到bbb上,实现安装系统到emmc

### 安装到emmc

接之前的livecd模式,之前以debian/temppwd登陆.

#### 复制镜像到bbb上

通过以太网.`sudo ifconfig`查看bbb的ip


	eth0      Link encap:Ethernet  HWaddr 1c:ba:8c:9f:d2:df  
		      inet addr:192.168.1.107  Bcast:255.255.255.255  Mask:255.255.255.0
		      inet6 addr: fe80::1eba:8cff:fe9f:d2df/64 Scope:Link
		      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
		      RX packets:40230 errors:0 dropped:0 overruns:0 frame:0
		      TX packets:14412 errors:0 dropped:0 overruns:0 carrier:0
		      collisions:0 txqueuelen:1000 
		      RX bytes:60678623 (57.8 MiB)  TX bytes:983048 (960.0 KiB)
		      Interrupt:40 

	lo        Link encap:Local Loopback  
		      inet addr:127.0.0.1  Mask:255.0.0.0
		      inet6 addr: ::1/128 Scope:Host
		      UP LOOPBACK RUNNING  MTU:65536  Metric:1
		      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
		      TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
		      collisions:0 txqueuelen:0 
		      RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

	usb0      Link encap:Ethernet  HWaddr 26:16:34:0b:af:d7  
		      inet addr:192.168.7.2  Bcast:192.168.7.3  Mask:255.255.255.252
		      UP BROADCAST MULTICAST  MTU:1500  Metric:1
		      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
		      TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
		      collisions:0 txqueuelen:1000 
		      RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

我就通过eth0上传了,usb0也是可以的.

在本机上

	zodiac1111@debian:music$ scp 'bone-debian-7.5-2014-05-06-2gb.img.xz' debian@192.168.1.107:
	The authenticity of host '192.168.1.107 (192.168.1.107)' can't be established.
	ECDSA key fingerprint is 2d:9d:c5:07:1d:54:65:2a:cc:ec:ac:8e:94:ee:ba:f1.
	Are you sure you want to continue connecting (yes/no)? yes
	Warning: Permanently added '192.168.1.107' (ECDSA) to the list of known hosts.
	debian@192.168.1.107's password: 
	bone-debian-7.5-2014-05-06-2gb.img.xz      100%  162MB   2.8MB/s   00:57 

在bbb上看看是不是在家目录上了

	debian@arm:~$ ls
	bin  bone-debian-7.5-2014-05-06-2gb.img.xz

查看bbb上磁盘结构(我这里正好两个都是2g的,看挂载点就区分出来了)

	debian@arm:~$ lsblk
	NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
	mmcblk1boot0 179:16   0     1M  1 disk 
	mmcblk1boot1 179:24   0     1M  1 disk 
	mmcblk0      179:0    0   1.8G  0 disk  <-这是livecd挂载的
	├─mmcblk0p1  179:1    0    96M  0 part /boot/uboot
	└─mmcblk0p2  179:2    0   1.6G  0 part /
	mmcblk1      179:8    0   1.8G  0 disk  <-这个是bbb上本身的emmc,我之前刷过,所以结构颇像
	├─mmcblk1p1  179:9    0    96M  0 part 
	└─mmcblk1p2  179:10   0   1.6G  0 part 

在bbb上解压镜像并写入到mmc中

	debian@arm:~$ xz -cd bone-debian-7.5-2014-05-06-2gb.img.xz > /dev/mmcblk1
	# 需要一段时间,取决于SD的读取速度

过了大约 3-5分钟,完成输入`sudo poweroff`关机,电源灯熄灭.然后移除microSD卡

再次启动,按一下电源按钮,就从mmc启动了debian

# debian上的i2c

参考 https://groups.google.com/d/msg/beagleboard/7reYt7Tmdjs/5LLRvCTPNuoJ


    echo BB-I2C1 > /sys/devices/bone_capemgr.????/slots

# 其他

砖了? [修砖不擦除eMMC](http://hipstercircuits.com/unbrick-beaglebone-black-without-erasing-emmc/)

## bbb i2c adxl345

[[/dev/driver-adxl345]] 后面部分记录在debian下使用i2c读取adxl345 
# 参考资料
* 国内论坛 http://bbs.eetop.cn/forum-266-1.html
* 安装xfce桌面环境 http://www.huement.com/blog/?p=971
* debian 无线网卡 http://my.oschina.net/robeer/blog/207715 [自己整理](bbb-debian-wifi)
* 技术博客cape相关 http://papermint-designs.com/community/node/338
* 技术博客硬件cape相关 http://azkeller.com/blog/?p=62
* wiki elinux http://elinux.org/Beagleboard:BeagleBoneBlack
* 全面入门简要指南 http://eewiki.net/display/linuxonarm/BeagleBone+Black
* U-Boot on the BBB http://www.crashcourse.ca/wiki/index.php/U-Boot_on_the_BBB
* beaglebone 玩法项目 http://learn.adafruit.com/category/beaglebone
* 谷歌论坛 https://groups.google.com/forum/#!forum/beaglebone
* 中文,个人博客.BBB相关使用笔记分享 http://blog.csdn.net/wyt2013/
* 第4周开始硬件部分使用beaglebone black教学 https://class.coursera.org/conrob-002/lecture/150
# apt-get无效

```bash
W: A error occurred during the signature verification. The repository is not updated and the previous
 index files will be used. GPG error: http://security.debian.org wheezy/updates Release: The followin
g signatures were invalid: KEYEXPIRED 1587841717

W: GPG error: http://ftp.us.debian.org wheezy Release: The following signatures were invalid: KEYEXPI
RED 1587841717 KEYEXPIRED 1557241909
W: A error occurred during the signature verification. The repository is not updated and the previous
 index files will be used. GPG error: http://ftp.us.debian.org wheezy-updates Release: The following 
signatures were invalid: KEYEXPIRED 1587841717
```

查看系统时间

```bash
root@arm:~# date
Wed Nov 15 01:31:20 UTC 2028
root@arm:~# date
```
呵呵呵

更新一下时间
```
ntpdate time.apple.com
```

* 启动是挂载硬盘,可以在`/etc/rc.local`中添加

   mount -t ext4  /dev/sda1 /mnt

