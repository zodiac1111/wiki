# 虚拟midi键盘

Virtual MIDI Piano Keyboard

> http://forum.ubuntu.org.cn/viewtopic.php?f=122&t=282545

[[http://tedfelix.com/linux/linux-midi.png]]

1. vmpk:MIDI信号发生器，负责将按键转换成 MIDI 信号
2. MIDI信号通过 alsa/jack 被投递到音源（Synthesizer，又称合成器，用于合成、产生对应频率的声音）或序列器（Sequencer，用于录制MIDI信号，需要时把录制好的信号传递到音源）
jack/alsa 好像都是要配置“MIDI连接”的（alsa 可以用 aconnect，jack 可以用 QJackCtl）

# midi键盘(F37)

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


参考2

* [小汤普森钢琴简易教程5](http://www.youku.com/playlist_show/id_17294924.html)


