# i2c相关

## 设配规则

udev设配规则:https://xgoat.com/wp/2008/01/29/i2c-device-udev-rule/

```
KERNEL=="i2c-[0-9]*", GROUP="i2c"
```

## 一些参考 

I2C on a Linux based embedded system(pdf):  
http://home.deib.polimi.it/bellasi/lib/exe/fetch.php?media=students:giginghelli_finalreport.pdf

Behavioural analysis of an I2C Linux Driver(pdf) i2c linux 驱动行为分析,研究性文章:  
http://alexandria.tue.nl/repository/books/653250.pdf

I2C Drivers, Part I(2003年)简单驱动实例:  
http://www.linuxjournal.com/article/7136

Interfacing with I2C Devices(http://elinux.org/)主要是应用层:  
http://elinux.org/Interfacing_with_I2C_Devices

可用设配:  
adxl345加速度传感器: http://www.analog.com/static/imported-files/data_sheets/ADXL345.pdf   
adc  http://wiki.analog.com/resources/tools-software/linux-drivers/iio-adc/ad7998

i2c smbus幻灯片(美国亚利桑那州立大学):  
http://rts.lab.asu.edu/web_438/CSE438_598_slides_yhlee/438_6_Linux_I2C_SMBus.pdf

i2c wiki
https://i2c.wiki.kernel.org/index.php/Main_Page

i2c stub测试桩 (a very simple fake I2C/SMBus driver)
http://git.kernel.org/?p=linux/kernel/git/torvalds/linux-2.6.git;a=blob;f=Documentation/i2c/i2c-stub

Upgrading I2C chip drivers to the 2.6 driver mode,从芯片驱动升级到2.6的驱动模型.
http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/i2c/upgrading-clients

Writing kernel drivers for I2C or SMBus devices(写内核驱动,kernel文档)
http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/i2c/writing-clients

How to instantiate I2C devices (如何实现设配,内核文档)
http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/i2c/instantiating-devices

Conventions for use of fault codes in the I2C/SMBus stack(故障代码和错误代码相关)
http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/i2c/fault-codes

I2C bus multiplexing(多路)
https://i2c.wiki.kernel.org/index.php/I2C_bus_multiplexing

应用层的一个简单例子:应该可以配合测试桩进行测试<- 待测试 参见:linux/Documentation/i2c/dev-interface
http://bunniestudios.com/blog/images/infocast_i2c.c

中文:2.6.32新型模型例子
http://ticktick.blog.51cto.com/823160/971738
## 个人理解

两种方式:

普通i2c 和 SMbus <--推荐

SMbus是普通i2c的子集,支持普通i2c就可以支持SMbus,看实现.有些比较奇葩的可能只能使用普通i2c实现

i2c 驱动 设备

```
/sys/bus/i2c/devices 设配
/sys/bus/i2c/drivers 驱动

/dev/i2c-0
/dev/i2c-1

/dev/i2c/0
/dev/i2c/1
```

重要文件

```
include/linux/i2c.h 结构体  i2c_adapter(适配器) i2c_algorithm(算法)
```

目录结构:
```
$ tree /sys/class/i2c-adapter/
/sys/class/i2c-adapter/
`-- i2c-0
    |-- device -> ../../../devices/legacy/i2c-0
    `-- driver -> ../../../bus/i2c/drivers/i2c_adapter
```

算法例子:
```
static struct i2c_algorithm tiny_algorithm = {
    .name           = "tiny algorithm",
    .id             = I2C_ALGO_SMBUS,
    .smbus_xfer     = tiny_access,
    .functionality  = tiny_func,
};
```

# 调试

## 使用 i2c-stub 测试桩

测试桩驱动源代码,driver/i2c/busses/i2c-stub.c,测试程序支持smbus总线

编译i2c-stub 测试桩模块,加载模块
```
insmod i2c-stub.ko chip_addr=0x50
```

*  i2c-stub.ko 测试桩模块,用于测试,仅实现了部分的驱动函数
*  chip_addr 芯片地址,"Chip addresses (up to 10, between 0x03 and 0x77)" i2c-stub.c:line 36.

```
cat /proc/kmesg
<6>i2c-stub: Virtual chip at 0x50 //虚拟芯片地址
<7>i2c i2c-1: adapter [SMBus stub driver] registered
<7>i2c-dev: adapter [SMBus stub driver] registered as minor 1
```
查看是不是新增了一个设备
```
ls /dev/i2c*
```


内核开启调试信息,查看调试桩的反馈
```
cat /proc/kmesg
<7>i2c i2c-1: ioctl, cmd=0x703, arg=0x1d //设备地址
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbeeabcf8 
<7>i2c i2c-1: smbus byte - addr 0x1d, wrote 0x10.  //写10,欲读取寄存器地址 (byte)
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbeeabcf8
<7>i2c i2c-1: smbus byte - addr 0x1d, read  0xab.  //读刚才的10,地址,值为ab (byte)
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbeeabcf8
<7>i2c i2c-1: smbus byte data - addr 0x1d, wrote 0xab at 0x10. //在10处写ab  (byte data)
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbeeabcf8
<7>i2c i2c-1: smbus byte - addr 0x1d, wrote 0x10. //写10,欲读取的地址 (byte)
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbeeabcf8
<7>i2c i2c-1: smbus byte - addr 0x1d, read  0xab. //读取,得到值ab  (byte)
```