# mini2440 uboot

使用buildroot编译uboot.


* [目录](customize-mini2440-softwave) 


1. `make menuconfig` 
2. Bootloaders 
3. U-Boot 
4. U-Boot board name `smdk2410` 硬件密切相关  
 在`output/build/uboot-2013.04/board`目录文件夹名称

## 在原有的vivi bootloader情况下安装uboot.

mini2440版本的uboot http://wiki.linuxmce.org/index.php/Mini2440

在linux通过usb下载uboot到mini2440开发板上的软件 https://github.com/changbindu/dnw-linux

1. mini2440 官方nor模式进入vivi bootloader.
2. a
3. host上执行 `dnw -a 33f80000 uboot.bin`  # 33f80000 这个数字有硬件决定
4. 开发板关机,切换到nand模式开机
5. minicom上任意键,停在uboot界面
6. 今后从nand flash启动就是都由uboot引导了.

可能是我的nand flash有问题,saveenv时只能停在擦出界面,所以在`/include/configs/mini2440.h`文件中修改了一些默认的参数,如主机和目标板的IP地址等.

http://wiki.linuxmce.org/index.php/Mini2440中提到的usb下载工具无效的情况下可以使用dnw按照以上步骤试试.如果wiki页面提到的方法有下,则推荐使用其方式.

## 在已经有uboot的情况下安装/更新 uboot

推荐 Tekkaman 的 http://u-boot-all-in-one.googlecode.com/files/mini2440%E4%B9%8BU-boot%E7%A7%BB%E6%A4%8D%E8%AF%A6%E7%BB%86%E6%89%8B%E5%86%8C-20100419.pdf 