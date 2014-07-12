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

### 再来一次的记录

* 从[这里](http://elinux.org/Beagleboard:Debian_On_BeagleBone_Black)看到
* 下载 img.xz 文件而不是 .tar.xz文件,比如我下载`bone-debian-7.5-2014-05-06-2gb.img.xz`
* 解压这个文件,官方说用7z,我用debian自带的也行.
* 得到文件:`bone-debian-7.5-2014-05-06-2gb.img`
* window磁盘写入工具,我用debian自带的"磁盘"工具:附件>磁盘>
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

