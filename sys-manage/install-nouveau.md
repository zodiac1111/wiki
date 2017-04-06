
参考 http://askubuntu.com/questions/12937/remove-nvidia-driver-and-go-back-to-nouveau


To reconfigure xorg.conf. Move your current /etc/X11/xorg.conf. If things go wrong you might need it later again:

```
sudo mv /etc/X11/xorg.conf /etc/X11/xorg.conf.BACKUP
```
The following steps will install the nouveau-driver on configure the xserver accordingly:

```
sudo apt-get install nouveau-firmware
sudo dpkg-reconfigure xserver-xorg
```


Go following the screen steps, answering the wizard questions and you should able to restore or reconfigure to previous Nouveau state.


