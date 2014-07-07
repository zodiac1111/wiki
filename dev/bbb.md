# BeagleBone Black 相关笔记

# 准备
##  硬件获得
* 购买: chipsee 官方指定中国代理商;  http://www.chipsee.com/  淘宝 http://item.taobao.com/item.htm?id=20044299382
* 中国版BB-Black 英蓓特 http://www.embedinfo.com/

# 安装debian

* 1 [简单博客](http://blogs.bu.edu/mhirsch/2013/11/install-debian-7-to-emmc-internal-flash-drive-of-beaglebone-black/) 安装到eMMC ,他的文件是[这里](https://rcn-ee.net/deb/microsd/wheezy/) 下的,下bone * img.xz
* 1.1 [安装debian/ubuntu](http://www.gigamegablog.com/2012/09/03/ubuntu-on-the-beaglebone-enabling-analog-in-pwm-i2c-and-spi/)
* 2 [adafruit的专题](http://learn.adafruit.com/downloads/pdf/beaglebone-black-installing-operating-systems.pdf)
* 3 [elinux.org bbb安装debian](http://elinux.org/Beagleboard:Debian_On_BeagleBone_Black)
* 4 [比较全面,从bootloader,交叉编译器到根文件系统都有](http://eewiki.net/display/linuxonarm/BeagleBone+Black)

A: 使用sd卡引导,不安装到flash

B: 安装到flash (我的选择)

主要步骤参考1, 镜像在3上找到

* 复制到sd卡
* 把sd卡插到bbb上,While holding down the 'boot' button, apply power to the board. Continue to hold the 'boot' button until the USER LEDs begin to flash 用户名密码 debian/temppwd
* 启动sd卡上的系统(类似livecd)
* 从host复制镜像到bbb上 (这个livecd内容小,所有从host安装,类似livecd安装到硬盘)
* 在bbb上,解压镜像到块设备上 lsblk ` mmcblk1      179:8    0   1.8G  0 disk` 类似镜像恢复


# debian上的i2c

参考 https://groups.google.com/d/msg/beagleboard/7reYt7Tmdjs/5LLRvCTPNuoJ


    echo BB-I2C1 > /sys/devices/bone_capemgr.????/slots


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