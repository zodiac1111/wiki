
https://wiki.debian.org/BluetoothUser/a2dp


a2dp

1. 蓝牙
2. 音频控制

安装一些软件

```
apt-get install pulseaudio pulseaudio-module-bluetooth pavucontrol bluez-firmware
```

重启服务

```
systemctl restart bluetooth
```

配对

桌面托盘gui,gnome提供`gnome-bluetooth` 其他可以使用 `blueman`