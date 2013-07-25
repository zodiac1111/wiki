# usbasp 笔记

# 硬件

硬件来源(淘宝) http://tradearchive.taobao.com/trade/detail/tradeSnap.htm?spm=a1z09.2.9.147.BquMN6&tradeID=79509563401088

分析发现改下载器基本基于 http://www.fischl.de/usbasp/ 这个. 

按照官方说明:

>On Linux and MacOS X **no** kernel driver is needed.

不需要驱动,很好.在本人的fedora18上也测试通过.

# 软件

> [AVRDUDE](http://www.nongnu.org/avrdude/) is an utility to download/upload/manipulate the ROM and EEPROM contents of AVR microcontrollers using the in-system programming technique (ISP).

支持多种下载器硬件(上面这种也支持).中文介绍: http://blog.21ic.com/user1/69/archives/2005/1551.html .

# 测试

## 需要root权限

没有权限,需要root

```
$ avrdude -cusbasp -pm8 -F
avrdude: Warning: cannot open USB device: Permission denied
avrdude: error: could not find USB device "USBasp" with vid=0x16c0 pid=0x5dc

avrdude done.  Thank you.
```

## 下载器硬件测试

仅连接下载器,不连接开发板,测试

```
# avrdude -cusbasp -pm8 -F

avrdude: warning: cannot set sck period. please check for usbasp firmware update. 
avrdude: error: programm enable: target doesn't answer. 1 
avrdude: initialization failed, rc=-1
avrdude: AVR device initialized and ready to accept instructions
avrdude: Device signature = 0x000000
avrdude: Yikes!  Invalid device signature.
avrdude: Expected signature for ATMEGA8 is 1E 93 07

avrdude done.  Thank you.
```

* `-cusbasp` 指定usbasp下载器类型
* `-pm8` 指定单片机类型,这里什么类型不重要,因为根本没有连接单片机,这一步仅测试下载器
* `-F` 强制执行,因为没有连接单片机,所以会显示错误,所以强制执行.

这时会看到代表数据的红色led闪烁一下.基本表示软硬件连接成功.

# 连上单片机测试

这个时候连上mega8单片机最小系统.再次执行
```
avrdude -cusbasp -pm8 
```
得到的显示:

```
avrdude: warning: cannot set sck period. please check for usbasp firmware update.
avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9307

avrdude: safemode: Fuses OK

avrdude done.  Thank you.
avrdude: warning: cannot set sck period. please check for usbasp firmware update.
avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9307

avrdude: safemode: Fuses OK

avrdude done.  Thank you.
```
恩,正确识别的单片机 :D