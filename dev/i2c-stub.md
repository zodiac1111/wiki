# i2c测试桩(stub)

* 源代码: driver/i2c/busses/i2c-stub.c 
* 官方的使用指南: Documentation/i2c/i2c-stub (This module is a very simple fake I2C/SMBus driver .)

开发i2c驱动强烈建议读完这个i2c文件夹下的文档.网上很多资料都是基于此的.[这里](i2c)有些我搜集的资料.

# 背景

在没有i2c设备时在应用层开发i2c的程序,stub实现了简单的读写功能,可以视为一个虚拟的从设备,则加载这个模块就可以用于调试应用层的一般逻辑流程,相当与一个测试软件.如modbus也有类似的软件模拟设备,方便软件层面的调试.

另一方面这也是个驱动的helloworld程序,结构简单,可以参考来开发i2c驱动.非常有用.

# 使用

## 流程概述

1. 加载模块
2. 使用虚拟的设备文件调试应用层程序

## 详细准备

### 获得模块

配置编译模块

`make menuconfig` 时
```bash
Device Drivers  ---> 
   I2C support  --->
      I2C Hardware Bus support  --->
         I2C/SMBus Test Stub 
```

选择编译成为模块.

同时强烈建议选择一下几项,输出调试信息,非常有帮助:
```bash
Device Drivers  ---> 
   I2C support  --->
      * I2C Core debugging messages
      * I2C Algorithm debugging messages
      * I2C Bus debugging messages
      * I2C Chip debugging messages 
```
与往常一样编译(内核),下载,重启.

### 使用

加载内核:
```bash
insmod i2c-stub.ko chip_addr=0x1d
```
*  i2c-stub.ko 测试桩模块,用于测试,仅实现了部分的驱动函数
*  chip_addr 芯片地址,"Chip addresses (up to 10, between 0x03 and 0x77)" i2c-stub.c:line 36.

这时候如果内核编译了上一步的调试信息的话:

```bash
cat /proc/kmesg
<6>i2c-stub: Virtual chip at 0x1d //<-虚拟芯片地址(虚拟的从设备)
<7>i2c i2c-1: adapter [SMBus stub driver] registered
<7>i2c-dev: adapter [SMBus stub driver] registered as minor 1
```

查看是不是新增了一个设备文件
```bash
ls /dev/i2c*
```
查看mod
```bash
[root@FriendlyARM plg]# lsmod
i2c_stub 2256 0 - Live 0xbf02a000

```
### 调试

编写自己的应用层的程序,调用是选择这个虚拟设备,以下仅供参考,我也是刚接触这方面.

```c
///@filename i2c-other.c
//通过i2c-stub调试应用层的驱动程序
#define ADDRESS 0x1D // stub测试桩地址,动过insmod的参数指定
#define DEV "/dev/i2c/1"
#include <stdio.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <linux/i2c-dev.h>
#include <linux/i2c.h>
#include <sys/ioctl.h>

int main(int argc, char* argv[])
{
	int fd;
	union i2c_smbus_data data;
	struct i2c_smbus_ioctl_data args;
	unsigned char reg =0x10; //读取(寄存器)地址
	int funcs;
	// 1  打开 ,从设备文件打开
	fd = open(DEV, O_RDWR);
	if (fd<0) {
		perror("open");
		return -1;
	}
	// 2 获得支持函数列表<不必须>但是可以知道很多有用的信息
	// 支持的函数返回,并保存在 funcs 中.
	if (ioctl(fd, I2C_FUNCS, &funcs)<0) {
		perror("获得支持函数列表");
		goto CLOSE;
	}
	// 2.1 check for req funcs 通过 & 操作得到是不是支持这些函数.
	// 应用层可以判断什么函数能用,什么不能用.类似向下兼容的作用
	// 各种功能宏在<linux/i2c.h>中定义
	if ((funcs&I2C_FUNC_SMBUS_READ_BYTE)==0) {
		printf("Error:I2C_FUNC_SMBUS_READ_BYTE "
				"function is required.\n");
	}
	// 3 设置从设备地址
	if (ioctl(fd, I2C_SLAVE, ADDRESS)!=0) {
		perror("ioctl");
		goto CLOSE;
	}
	//读
	args.read_write = I2C_SMBUS_READ;	//读
	args.command = reg;			//地址
	args.size = I2C_SMBUS_BYTE_DATA;	//1个字节的数据
	args.data = &data;			//保存数据
	if (ioctl(fd, I2C_SMBUS, &args)!=0) {
		perror("ioctl");
		goto CLOSE;
	}
	printf("0x%x 读取结果: 0x%x\n", reg, data.byte);
	//写
	args.read_write = I2C_SMBUS_WRITE;	//写
	args.command = reg;			//地址
	args.size = I2C_SMBUS_BYTE_DATA;	//1个字节数据
	data.byte = 0xab;			//数据
	args.data = &data;			//数据联合体
	if (ioctl(fd, I2C_SMBUS, &args)!=0) {
		perror("ioctl");
	}
	printf("在 0x%x 处写入 0x%x \n", args.command, data.byte);
	//读
	args.read_write = I2C_SMBUS_READ;
	args.command = reg;
	args.size = I2C_SMBUS_BYTE_DATA;
	args.data = &data;
	if (ioctl(fd, I2C_SMBUS, &args)!=0) {
		printf("再次读取ioctl错误");
		goto CLOSE;
	}
	printf("再次读取 0x%x 结果: 0x%x\n", reg, data.byte);
	CLOSE:
	close(fd);
	return 0;
}

```
编译
```bash
arm-linux-gcc i2c-other.c -o app -Wall
```
运行应用程序
```bash
[root@FriendlyARM plg]# ./app 
0x10读取结果: 0x0
在 0x10 处写入 0xab 
再次读取 0x10 结果: 0xab
```

同时查看debug信息(需要上面编译内核时选择):
```bash
cat /proc/kmesg
<7>i2c i2c-1: ioctl, cmd=0x705, arg=0xbe928cf4
<7>i2c i2c-1: ioctl, cmd=0x703, arg=0x1d
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe928cf8
<7>i2c i2c-1: smbus byte data - addr 0x1d, read  0x00 at 0x10.
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe928cf8
<7>i2c i2c-1: smbus byte data - addr 0x1d, wrote 0xab at 0x10.
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe928cf8
<7>i2c i2c-1: smbus byte data - addr 0x1d, read  0xab at 0x10.
```

配合i2c-stub.c的源代码对理解驱动有很大的帮助.