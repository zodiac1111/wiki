# Guest机闪退

一句话解决方案:安装最新的VirtualBox扩展包:[官网](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html#extpack)

运行环境

* host debian

```
Linux debian 3.16.0-4-amd64 #1 SMP Debian 3.16.7-2 (2014-11-06) x86_64 GNU/Linux
```

* guest Win7 Win8.1 Win10技术预览版.
* VirtualBox 4.3.18_Debian r96516
* VittualBox扩展包 **Oracle_VM_VirtualBox_Extension_Pack-4.3.18-96516.vbox-extpack**

在VittualBox虚拟机的"设置"->"usb设备"标签页,勾选"启用USB控制器"->"启用USB2.0(EHCI)控制器"

表现:Guest机启动闪退,运行时蓝屏,插入usb设备蓝屏,usb设备识别,但是驱动安装出错,在Guset机的"设备管理器"中显示一个黄色小感叹号.并且设备无法使用全部功能,错误代码10. 设备启动参数错误.

比如一些u盘,手机等无法正常工作,一些低速简单设备比如usb转串,usb鼠标等可以工作.
