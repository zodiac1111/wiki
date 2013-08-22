# Debian下播放.ape音乐文件

http://deb-multimedia.org/

Rhythmbox/Banshee 播放ape(monkey's audio)格式音乐文件.

Rhythmbox/Banshee都是通过gstreamer来解码各种格式的音乐.

另外一类播放器则不是通过gstreamer来解码的,如mplayer/vlc/decibel audio player/qmmp这一类.

所以表现出就是使用不同类的播放器可能有的可以播放,有的却不能播放/播放错误.

Debiana 7 Wheezy 安装以下包 

```
gstreamer1.0-libav 
gstreamer0.10-ffmpeg
```
默认情况下可以解析ape文件,但是时间解析错误,可能解析成一个很大的数字,音乐不能意义对应.

如`gstreamer1.0-libav `没有安装,可以播放完成的文件,时间也对.但是有些声音明显的有爆破感.人耳可以轻易分辨.同时使用mplayer类播放器播放同一个ape文件,没有声音爆破的感觉.排除硬件和文件本身音质的问题.锁定在解码程序上.

http://pkgs.org/search/?keyword=gstreamer0.10-ffmpeg

第三方插件:

https://wiki.gnome.org/RhythmboxPlugins/ThirdParty#Tray_Icon

最小化到通知区域,精简模式
