# kindle自定义屏保

仅支持png

需求:

1. 越狱
2. usbnetwork或者其他可以修改/usr目录下文件的方法

步骤:

1. 越狱
2. 安装usbnetwork
3. 链接上kindle(wifi或usb)
4. 执行`mntroot rw`使之可写
5. `sftp://root@192.168.XXX.XXX/usr/share/blanket/`目录下

一些文件/目录解释:

1. `screensaver`屏保文件夹
  * 命名必须以 `bg_medium_ss<序号>.png`
2. `bg_medium_defaultbg.png` 启动屏幕

图片浏览器:`imageViewer.sh` http://wiki.mobileread.com/wiki/Kindle_Touch_Hacking#Image_Viewer