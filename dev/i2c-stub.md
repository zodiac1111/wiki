# i2c测试桩(stub)

* 源代码: driver/i2c/busses/i2c-stub.c 
* 官方的使用指南: Documentation/i2c/i2c-stub (This module is a very simple fake I2C/SMBus driver .)

# 背景

在没有i2c设备时在应用层开发i2c的程序,stub实现了简单的读写功能,可以视为一个虚拟的从设备,则加载这个模块就可以用于调试应用层的一般逻辑流程,相当与一个测试软件.如modbus也有类似的软件模拟设备,方便软件层面的调试.

# 使用

## 流程概述

1. 加载模块
2. 使用虚拟的设备文件调试应用层程序

## 详细准备

### 获得模块

配置编译模块

`make menuconfig` 时
```
Device Drivers  ---> 
   I2C support  --->
      I2C Hardware Bus support  --->
         I2C/SMBus Test Stub 
```

选择编译成为模块.

同时强烈建议选择一下几项,输出调试信息,非常有帮助:
```
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
```
insmod i2c-stub.ko chip_addr=0x1d
```
*  i2c-stub.ko 测试桩模块,用于测试,仅实现了部分的驱动函数
*  chip_addr 芯片地址,"Chip addresses (up to 10, between 0x03 and 0x77)" i2c-stub.c:line 36.

这时候如果内核编译了上一步的调试信息的话:

```
cat /proc/kmesg
<6>i2c-stub: Virtual chip at 0x1d //<-虚拟芯片地址(虚拟的从设备)
<7>i2c i2c-1: adapter [SMBus stub driver] registered
<7>i2c-dev: adapter [SMBus stub driver] registered as minor 1
```

查看是不是新增了一个设备文件
```
ls /dev/i2c*
```
查看mod
```
[root@FriendlyARM plg]# lsmod
i2c_stub 2256 0 - Live 0xbf02a000

```
### 调试

编写自己的应用层的程序,调用是选择这个虚拟设备,以下仅供参考,我也是刚接触这方面.

```
// i2c 应用层调试,读写挂载在i2c总线上的eeprom file:i2c-other.c
// 设置从设备,mini2440 参考其给的例子,完全在应用层
// 驱动层到 /dev/i2c/0 这个设备的过程完全没有涉及,
// 对于eeprom配合原始的i2c -w 和 i2c -r 检查结果
//
//切换 EEPROM 则1 其他则测试桩
#define EEPROM 0
#if EEPROM==1
#define ADDRESS 0x50 // eeprom的地址
#define DEV "/dev/i2c/0"
#else
#define ADDRESS 0x1D // eeprom的地址
#define DEV "/dev/i2c/1"
#endif
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
// i2c
#include <unistd.h>
#include <fcntl.h>
#include <linux/i2c-dev.h>
#include <linux/i2c.h>
#include <sys/ioctl.h>
int main(int argc,char* argv[])
{
	int fd;
	int ret;
	char buf[10];
	union i2c_smbus_data data;
	struct i2c_smbus_ioctl_data args;
	int r ;
	//打开 ,从设备打开
	if(argc==2){
		fd = open(argv[1], O_RDWR );
	}else{
		fd = open( DEV, O_RDWR );
	}
	if(fd<0){
		perror("打开:");
		printf("fd=%d\n",fd);
		return -1;
	}
	ret=ioctl( fd, I2C_SLAVE, ADDRESS );
	if(ret<0){
		perror("ioctl:");
		printf("i2c_slave=%d\n",I2C_SLAVE);
		goto CLOSE;
	}
	// *****************  读取 ********************** //
	//读=写地址,读内容
	buf[0]=0x10;//register,对于eeprom就是地址
	//先写地址
	args.read_write = I2C_SMBUS_WRITE; //写
	args.command = 0x10; //地址
	args.size = I2C_SMBUS_BYTE;//大小一比特
	args.data = NULL;//没有数据
	r=ioctl(fd,I2C_SMBUS,&args);
	if(r<0){
		printf("r<0 r=%d\n",r);
	}
	//再访问,读
	args.read_write = I2C_SMBUS_READ; //读
	args.command = 0; //命令?
	args.size = I2C_SMBUS_BYTE;//大小一比特
	args.data = &data;//保存数据
	ret=ioctl(fd,I2C_SMBUS,&args);
	if(ret!=0){ //非零错误
		printf("读取ioctl错误");
		goto CLOSE;
	}
	//正确读取,数据保存在data中
	printf("0x%x读取结果: 0x%x\n",0x10,data.byte);
	// ******************* 写入 **********************  /
	//写
	args.read_write = I2C_SMBUS_WRITE; //写
	args.command = 0x10; //地址
	args.size = I2C_SMBUS_BYTE_DATA;//多个字节
	data.byte=0xab; //数据
	args.data =  &data;//数据联合体
	r=ioctl(fd,I2C_SMBUS,&args);
	if(r<0){
		printf("r<0 r=%d\n",r);
	}
	printf("在 0x%x 处写入 0x%x \n",args.command,data.byte);
	// ******  再次读取 *************************** ////
	//读=写地址,读内容
	buf[0]=0x10;//register,对于eeprom就是地址
	//先写地址
	args.read_write = I2C_SMBUS_WRITE; //写
	args.command = 0x10; //地址
	args.size = I2C_SMBUS_BYTE;//大小一比特
	args.data = NULL;//没有数据
	r=ioctl(fd,I2C_SMBUS,&args);
	if(r<0){
		printf("r<0 r=%d\n",r);
	}
	//再访问,读
	args.read_write = I2C_SMBUS_READ; //读
	args.command = 0; //命令?
	args.size = I2C_SMBUS_BYTE;//大小一比特
	args.data = &data;//保存数据
	ret=ioctl(fd,I2C_SMBUS,&args);
	if(ret!=0){ //非零错误
		printf("再次读取ioctl错误");
		goto CLOSE;
	}
	//正确读取,数据保存在data中
	printf("再次读取 0x%x 结果: 0x%x\n",0x10,data.byte);
CLOSE:
	close(fd);
	return 0;
}
```
编译
```
arm-linux-gcc i2c-other.c -o app -Wall
```
运行应用程序
```
[root@FriendlyARM plg]# ./app 
0x10读取结果: 0x0
在 0x10 处写入 0xab 
再次读取 0x10 结果: 0xab
```

同时查看debug信息(需要上面编译内核时选择):
```
cat /proc/kmesg
<7>i2c i2c-1: ioctl, cmd=0x703, arg=0x1d
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe9f8cf8
<7>i2c i2c-1: smbus byte - addr 0x1d, wrote 0x10.  //写10(地址)
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe9f8cf8
<7>i2c i2c-1: smbus byte - addr 0x1d, read  0x00. //读(10地址),得到至00
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe9f8cf8
<7>i2c i2c-1: smbus byte data - addr 0x1d, wrote 0xab at 0x10. //向10地址写入 ab
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe9f8cf8
<7>i2c i2c-1: smbus byte - addr 0x1d, wrote 0x10. //写10(地址)
<7>i2c i2c-1: ioctl, cmd=0x720, arg=0xbe9f8cf8
<7>i2c i2c-1: smbus byte - addr 0x1d, read  0xab. //读(10地址),得到值ab
```