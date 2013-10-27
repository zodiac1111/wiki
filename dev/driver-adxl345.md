# adxl345应用层驱动(i2c)

在mini2440上通过i2c使用adxl345三轴加速度传感器.使用s3c2440自带的i2c平台驱动.在应用层实现驱动.因为我还没有搞清楚i2c/smbus.

可以最简单的实现读取三轴加速度.比较丑陋正在完善中,别期待其与[官方的adxl345驱动](http://wiki.analog.com/resources/tools-software/linux-drivers/input-misc/adxl345)相比.

暂时缺陷:未实现多字节读取,每次只能操作一个字节.加速度数据高字节和低字节数值仍然有疑虑.

## 硬件

需要连接好adxl345和mini2440的线路.

电源,地,数据线,clk线

## 内核

不用任何改动,因为不是内核层的驱动.只要mini2440支持平台(s3c2440)的i2c支持即可.测试能和原来的eeprom通讯应该就没问题.

## 应用层

最简单文件,相当于一个helloworld程序,验证adxl345正常,验证连线正常.

### helloworld

功能:读取芯片id:DEVID[0x00]
```
///@filename adxl345-helloworld.c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/ioctl.h>
#include <linux/i2c-dev.h>
#include <linux/i2c.h>
#define ADDRESS 0x53 		// adxl345地址
#define DEV "/dev/i2c/0" 	//设配文件,与原先的eeprom一样,通过地址区分
#define DEVID 	0x00 		// adxl345器件id的寄存器地址,见数据手册
unsigned char adxl_read(int fd, unsigned char reg);
int main(int argc, char* argv[])
{
	int fd;
	int ret = 0;
	// 1  打开 ,从设备打开
	fd = open(DEV, O_RDWR);
	if (fd<0) {
		perror("open:");
		return -1;
	}
	// 2 设置从设备地址
	ret = ioctl(fd, I2C_SLAVE, ADDRESS);
	if (ret<0) {
		perror("ioctl:");
		goto CLOSE;
	}
	//读取器件id
	ret = adxl_read(fd, DEVID);
	printf("器件ID[0x%02x]=0x%02x\n", DEVID, ret);
	CLOSE:
	close(fd);
	return 0;
}

unsigned char adxl_read(int fd, unsigned char reg)
{
	union i2c_smbus_data data; 		//数据联合体
	struct i2c_smbus_ioctl_data args;	//参数结构
	int ret;
	// 1. 写寄存器地址
	args.read_write = I2C_SMBUS_WRITE;	//写
	args.command = reg;			//地址
	args.size = I2C_SMBUS_BYTE;	 	//大小一字节
	args.data = NULL;     			//没有数据
	ret = ioctl(fd, I2C_SMBUS, &args);
	if (ret<0) {
		perror("ioctl[写入]");
	}
	// 2. 读数据 (配合上面,即读某寄存器地址)
	args.read_write = I2C_SMBUS_READ;	//读
	args.command = 0;			//命令?
	args.size = I2C_SMBUS_BYTE;		//大小一字节
	args.data = &data;			//保存数据
	ret = ioctl(fd, I2C_SMBUS, &args);
	if (ret!=0) {     //非零错误
		perror("ioctl[读取]");
		return 0xff;
	}
	return data.byte;
}
```
编译:
```
arm-linux-gcc adxl345-helloworld.c -o app -Wall
```
在mini2440开发板上运行:
```
[root@FriendlyARM plg]# ./app 
器件ID[0x00]=0xe5
```
如果参考[这里](i2c-stub)设置了一些i2c调试信息,则`cat /proc/kmsg` 可以看到
```
<7>i2c i2c-0: ioctl, cmd=0x703, arg=0x53
<7>i2c i2c-0: ioctl, cmd=0x720, arg=0xbecf1cf4
<7>i2c i2c-0: master_xfer[0] W, addr=0x53, len=1
<7>s3c-i2c s3c2440-i2c: START: 000000d0 to IICSTAT, a6 to DS
<7>s3c-i2c s3c2440-i2c: iiccon, 000000e0
<7>s3c-i2c s3c2440-i2c: STOP
<7>s3c-i2c s3c2440-i2c: master_complete 0
<7>i2c i2c-0: ioctl, cmd=0x720, arg=0xbecf1cf4
<7>i2c i2c-0: master_xfer[0] R, addr=0x53, len=1
<7>s3c-i2c s3c2440-i2c: START: 00000090 to IICSTAT, a7 to DS
<7>s3c-i2c s3c2440-i2c: iiccon, 000000e0
<7>s3c-i2c s3c2440-i2c: READ: Send Stop
<7>s3c-i2c s3c2440-i2c: STOP
<7>s3c-i2c s3c2440-i2c: master_complete 0
```

### 读取传感器数值