# chrome关联特定程序

参考:http://askubuntu.com/questions/108925/how-to-tell-chrome-what-to-do-with-a-magnet-link

环境: 

* Debian Wheezy
* Mate-desktop
* google-chrome 29
* xdg-open

例如关联`irc://`协议使用`xchat-gnome`程序打开

bash:`xdg-mime default xchat-gnome.desktop x-scheme-handler/irc`

其中

* `xchat-gnome.desktop`是 xchat 客户端的文件,一般路径`/usr/share/applications/`
* `x-scheme-handler/irc`irc协议名称

其他如电骡,mms等链接应该也可以这样.

来源处有更详细的参考.
