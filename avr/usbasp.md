# usbasp 笔记

# 硬件

硬件来源(淘宝) http://tradearchive.taobao.com/trade/detail/tradeSnap.htm?spm=a1z09.2.9.147.BquMN6&tradeID=79509563401088

分析发现改下载器基本基于 http://www.fischl.de/usbasp/ 这个. 

按照官方说明:

>On Linux and MacOS X **no** kernel driver is needed.

不需要驱动,很好.在本人的fedora18上也测试通过.

# 软件

avrdude 是一款下载设置软件,支持多种下载器硬件(上面这种也支持).中文介绍: http://blog.21ic.com/user1/69/archives/2005/1551.html .