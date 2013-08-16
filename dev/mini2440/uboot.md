# mini2440 uboot

使用buildroot编译uboot.


* [目录](customize-mini2440-softwave) 


1. `make menuconfig` 
2. Bootloaders 
3. U-Boot 
4. U-Boot board name `smdk2410` 硬件密切相关  
 在`output/build/uboot-2013.04/board`目录文件夹名称

mini2440版本的uboot http://wiki.linuxmce.org/index.php/Mini2440

在linux通过usb下载uboot到mini2440开发板上的软件 https://github.com/changbindu/dnw-linux

1. mini2440 官方nor模式进入vivi bootloader.
2. a
3. host上执行 `dnw -a 33f80000 uboot.bin`
4. 开发板关机,切换到nand模式开机
5. minicom上任意键,停在uboot界面

可能是我的nand flash有问题,saveenv时只能停在擦出界面,所以在`/include/configs/mini2440.h`文件中修改了一些默认的参数,如主机和目标板的IP地址等.