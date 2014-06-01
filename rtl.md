# 开源无线电

参考 http://blog.sina.com.cn/s/blog_67cdafe201014odm.html

# 在debian上

安装参考1提到的一些软件
```text
gr-air-modes
rtl-sdr
```
运行程序如果出现以下的提示:
```text
Kernel driver is active, or device is claimed by second instance of librtlsdr.
In the first case, please either detach or blacklist the kernel module
(dvb_usb_rtl28xxu), or enable automatic detaching at compile time.
```
则试试这个,来源: https://groups.google.com/d/topic/ultra-cheap-sdr/6_sSON94Azo (谷歌搜到的)
```bash
sudo rmmod dvb_usb_rtl28xxu rtl2832 
```