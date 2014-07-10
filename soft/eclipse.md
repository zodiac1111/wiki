# eclipse 

# 本地化 

eclipse babel 项目,将各个插件/组件 翻译成各国语言

> http://www.eclipse.org/babel/downloads.php

## 在eclispe中安装

help->install-> 输入上面的(不同的版本分别)
* http://download.eclipse.org/technology/babel/update-site/R0.12.0/luna
* http://download.eclipse.org/technology/babel/update-site/R0.12.0/kepler
* http://download.eclipse.org/technology/babel/update-site/R0.12.0/juno

回车,显示Pending,正在载入列表.因为是国外网站,速度可能很慢,或者由于国家防火墙的原因无法访问.

勾选 Babel Language Packs in Chinese (Simplified) 下一步,下一步,同意,完成.这时会在后台下载安装(还是由于网络原因可能失败).

安装完后回提示重启eclipse

## 下载离线包

按照官方说明下载各种包,放到指定文件夹下

# 启动错误

显示类似如下:

	OpenJDK 64-Bit Server VM warning: You have loaded library /home/zodiac1111/Downloads/eclipse/plugins/org.eclipse.equinox.launcher.gtk.linux.x86_1.1.200.v20130807-1835/eclipse_1506.so which might have disabled stack guard. The VM will try to fix the stack guard now.
	It's highly recommended that you fix the library with 'execstack -c <libfile>', or link it with '-z noexecstack'.

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“murrine”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

	(eclipse:28962): Gtk-WARNING **: 无法在模块路径中找到主题引擎：“pixmap”，

下载一下包 `pixmap` 

没用 = =

还是先不折腾了

