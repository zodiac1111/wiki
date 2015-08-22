# Virtual MIDI Piano Keyboard

> http://forum.ubuntu.org.cn/viewtopic.php?f=122&t=282545

midi键盘 (F37)

* vmpk 生成 midi
* timidity / timidity-daemon midi播放

```
    # 安装midi键盘 (鍵盤組)
    # 安装 timidity 音源器
    sudo apt-get install timidity-daemon
    # 使能转送
    timidity -iA -B2,8 -Os -EFreverb=0
```

vmpk,连接,允许转发xxx
vmpk,连接,输入F37
vmpk,连接,输出选择timidity


参考

* [Virtual MIDI Piano Keyboard setup](http://askubuntu.com/questions/34391/virtual-midi-piano-keyboard-setup)
* (http://tern.logdown.com/posts/109476)



小汤普森钢琴简易教程5
http://www.youku.com/playlist_show/id_17294924.html

