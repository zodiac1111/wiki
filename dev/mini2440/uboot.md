# mini2440 uboot

使用buildroot编译uboot.

* [目录](customize-mini2440-softwave) 

## 清单

* uboot源代码(任一)
 * buildroot 自动下载通用的uboot(未包含mini2440的配置) 推荐进阶玩家
 * 为mini2440[定制的uboot](http://wiki.linuxmce.org/index.php/Mini2440),推荐新手,或者对搭建的环境进行测试时使用.

* usb下载软件(任一)
 * [wiki](http://wiki.linuxmce.org/index.php/Mini2440)中提到的[s3c2410_boot_usb](http://mini2440.googlecode.com/files/s3c2410_boot_usb-20060807.tar.bz2),推荐使用,无法使用的情况下用后者.编译时可能需要`libusb-dev`usb开发包中的头文件.
 * [dnw-linux](https://github.com/changbindu/dnw-linux) linux的dnw.进阶使用,或者前者不能使用时.疑问参见"故障排除"

# 得到uboot.bin

## buildroot步骤 

1. `make menuconfig` 
2. Bootloaders 
3. U-Boot 
4. 修改配置文件以符合mini2440硬件的
5. U-Boot board name `mini2440` 硬件密切相关(上一步配置的)  
 在`output/build/uboot-2013.04/board`目录文件夹名称

若初学不知如何配置,推荐先使用下面定制的uboot源码编译方式,以测试环境是否搭建成功.

## uboot官方

查看[这里](http://git.denx.de/cgi-bin/gitweb.cgi?p=u-boot.git;a=commit;h=b77026225a319066eaaa11839121a273469a2cf4)显示新的支持mini2440配置的uboot以于2012年由Gabriel Huau <contact@huau-gabriel.fr>提交到官方repo,并且成功合并. 

```
ARM : Add support for MINI2440 (s3c2440).

Support of the MINI2440 board from FriendlyARM from
an old version of u-boot :
http://repo.or.cz/r/u-boot-openmoko/mini2440.git

Currently, supporting only boot from NOR.

Signed-off-by: Gabriel Huau <contact@huau-gabriel.fr>
```

阅读diff查看具体更新说明.

## 定制的uboot源码

参照[wiki](http://wiki.linuxmce.org/index.php/Mini2440)使用git从仓库中下载源码

```
mkdir uboot ; cd uboot
git clone git://repo.or.cz/u-boot-openmoko/mini2440.git
```

设置交叉编译器前缀

```
export CROSS_COMPILE=arm-none-linux-gnueabi- 
# 需要按照实际情况设置,可以指向之前编译出的交叉编译器绝对路径
# 如 设置为 /home/use1/buildroot/output/host/usr/bin/arm-linux- 
# 只要`前缀+gcc` 等能正确执行即可
```

配置及编译

```
cd mini2440
make mini2440_config
make all
```

最后在源代码根目录得到`u-boot.bin`文件.


# 下载uboot.bin 

## 在原有的vivi bootloader情况下安装uboot.

mini2440版本的uboot http://wiki.linuxmce.org/index.php/Mini2440

在linux通过usb下载uboot到mini2440开发板上的软件 https://github.com/changbindu/dnw-linux

1. mini2440 官方nor模式进入vivi bootloader.
2. a
3. host上执行 `dnw -a 33f80000 uboot.bin`  # 33f80000 这个数字由硬件决定
4. 开发板关机,切换到nand模式开机
5. minicom上任意键,停在uboot界面
6. 今后从nand flash启动就是都由uboot引导了.

可能是我的nand flash有问题,`saveenv`时只能停在擦出界面,所以在`/include/configs/mini2440.h`文件中修改了一些默认的参数,如主机和目标板的IP地址等.

http://wiki.linuxmce.org/index.php/Mini2440中提到的usb下载工具无效的情况下可以使用dnw按照以上步骤试试.如果wiki页面提到的方法有下,则推荐使用其方式.

## 在已经有uboot的情况下安装/更新 uboot

推荐 Tekkaman 的 http://u-boot-all-in-one.googlecode.com/files/mini2440%E4%B9%8BU-boot%E7%A7%BB%E6%A4%8D%E8%AF%A6%E7%BB%86%E6%89%8B%E5%86%8C-20100419.pdf 

# 故障排除

## dnw

1. 可能需要手动加载驱动 `insmod secbulk.ko`
2. `dnw.rules`文件陈旧可能不能实现非root用户操作,
 * 使用root用户(不推荐) 或
 * 修改/增加`/etc/udev/rules.d/dnw.rules`文件:
   ```
   SUBSYSTEMS=="usb", ATTRS{idVendor}=="5345", ATTRS{idProduct}=="1234", GROUP="users", MODE="0666"
   ```
   原因与avrdude相似,可以参照.

### 