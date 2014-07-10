# 定制键盘布局

Fedora18 X系统下自定义键盘布局.

* **终端**(Ctrl+Alt+F?)下不可用,不是X系统 (debian见下)
* flash也可能失效

## 参考
* <http://survivalbit.com/blog/article/custom-keyboard-layout-fedora-gnome-3/>
* <http://wiki.linuxquestions.org/wiki/Altering_or_Creating_Keyboard_Maps>
* <http://wiki.linuxquestions.org/wiki/Accented_Characters>

## 相关文件

* /usr/share/X11/xkb/symbols/custom_en_de
* /usr/share/X11/xkb/rules/base.xml
* /usr/share/X11/xkb/rules/base.lst

# 虚拟终端下

> https://answers.launchpad.net/ubuntu/+question/7803

In order to get the layout install under /usr/share/keymaps/, you should install console-data

    $ sudo apt-get install console-data

The installer should guide you through a wizard. If it is not the case, type:

    $ sudo dpkg-reconfigure console-data

and choose your layout.