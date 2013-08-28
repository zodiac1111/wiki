# Linux下dnw

地址 : [dnw-linux](https://github.com/changbindu/dnw-linux) (github)

1. 可能需要手动加载驱动 `insmod secbulk.ko`
2. `dnw.rules`文件陈旧可能不能实现非root用户操作,
 * 使用root用户(不推荐) 或
 * 修改/增加`/etc/udev/rules.d/dnw.rules`文件:
   ```
   SUBSYSTEMS=="usb", ATTRS{idVendor}=="5345", ATTRS{idProduct}=="1234", GROUP="users", MODE="0666"
   ```
   原因与avrdude相似,可以参照.