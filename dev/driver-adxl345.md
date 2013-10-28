# 在应用层驱动adxl345(i2c)

在mini2440(linux)上通过i2c使用adxl345三轴加速度传感器.使用s3c2440自带的i2c平台驱动.在应用层实现驱动.因为我还没有搞清楚i2c/smbus :(.

可以最简单的实现读取三轴加速度.比较丑陋正在完善中,别期待其与[官方的adxl345驱动](http://wiki.analog.com/resources/tools-software/linux-drivers/input-misc/adxl345)相比.

暂时缺陷:虽然能读到数据,但是对于adxl345操作流程不甚了解,各寄存器需要再详细阅读datasheet.

## 硬件

需要连接好adxl345和mini2440的线路.

VCC,GND,SDA,SCL

## 内核&驱动层

不用任何改动,因为不是内核层的驱动.只要mini2440支持平台(s3c2440)的i2c支持即可.测试能和原来的eeprom通讯应该就没问题.

使用驱动层驱动是`drivers/i2c/busses/i2c-s3c2410.c`s3c2440的平台驱动.

## 应用层

这个驱动是且仅是在应用层完成的,具体看下面两个最简示例吧.

### 你好,世界!

最简单文件,相当于一个helloworld程序,目的是验证adxl345芯片和连线正常.

功能:读取芯片id:DEVID[0x00]

#### 代码

```c
// i2c 应用层调试,读写挂载在i2c总线上的adxl345,
// 与eeprom公用一个设配文件
// 设置从设备,mini2440 参考其给的例子,完全在应用层
// 驱动层到 /dev/i2c/0 这个设备的过程完全没有涉及,
// 对于eeprom配合原始的i2c -w 和 i2c -r 检查结果
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
	// 3 操作:读取器件id
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
	// 读数据
	args.read_write = I2C_SMBUS_READ;	//读
	args.command = reg;			//读取的位置(寄存器地址)
	args.size = I2C_SMBUS_BYTE_DATA;	//1个字节的数据
	args.data = &data;			//保存数据到data联合体
	ret = ioctl(fd, I2C_SMBUS, &args);
	if (ret!=0) {     //非零错误
		perror("ioctl[读取]");
		return 0xff;
	}
	return data.byte;
}

```
#### 编译

```bash
arm-linux-gcc adxl345-helloworld.c -o app -Wall
```

#### 运行

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

功能:仅实现了能读三轴加速度.

#### 代码

大部分代码与上面一样,增加了一个写入一个字节的函数`adxl_write()`,一个读取2个字节数据的函数`adxl_read_short()`
```
///@filename adxl345-simplest-use.c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/ioctl.h>
#include <linux/i2c-dev.h>
#include <linux/i2c.h>
#include <unistd.h>
#define ADDRESS 0x53 		// adxl345地址
#define DEV "/dev/i2c/0" 	//设配文件,与原先的eeprom一样,通过地址区分
#define DEVID 	0x00 		// adxl345器件id的寄存器地址,见数据手册
#define POWER_CTL 	0x2D
#define DATAX0		0x32
#define DATAX1		0x33
#define DATAY0		0x34
#define DATAY1 		0x35
#define DATAZ0 		0x36
#define DATAZ1 		0x37
unsigned char adxl_read(int fd, unsigned char reg);
int adxl_write(int fd, unsigned char reg, unsigned char val);
signed short  adxl_read_short(int fd, unsigned char reg);
int main(int argc, char* argv[])
{
	int fd;
	int ret = 0;
	signed short int x,y,z;//三轴加速度
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
	// 3 操作:读取器件id
	ret = adxl_read(fd, DEVID);
	printf("器件ID[0x%02x]=0x%02x\n", DEVID, ret);
	// 4 设置->启动,具体参见芯片手册.
	//   这里仅设置有限的读取数值功能.其实太复杂的还没搞懂:P
	adxl_write(fd, POWER_CTL, 0x28);
	// 5 循环测量(读取三轴加速度)
	for (;;) {
		x = adxl_read_short(fd, DATAX0);
		y = adxl_read_short(fd, DATAY0);
		z = adxl_read_short(fd, DATAZ0);
		printf("x=%5d y=%5d z=%5d\n", x,y,z);
		usleep(100*1000);
	}
CLOSE:
	close(fd);
	return 0;
}

unsigned char adxl_read(int fd, unsigned char reg)
{
	union i2c_smbus_data data; 		//数据联合体
	struct i2c_smbus_ioctl_data args;	//参数结构
	int ret;
	// 读数据
	args.read_write = I2C_SMBUS_READ;	//读
	args.command = reg;			//读取的位置(寄存器地址)
	args.size = I2C_SMBUS_BYTE_DATA;	//1个字节的数据
	args.data = &data;			//保存数据到data联合体
	ret = ioctl(fd, I2C_SMBUS, &args);
	if (ret!=0) {     //非零错误
		perror("ioctl[读取]");
		return 0xff;
	}
	return data.byte;
}
int adxl_write(int fd, unsigned char reg, unsigned char val)
{
	union i2c_smbus_data data;
	struct i2c_smbus_ioctl_data args;
	int ret;
	data.byte = val;			//数据
	args.read_write = I2C_SMBUS_WRITE;	//写[与上面读相对应]
	args.command = reg;			//地址
	args.size = I2C_SMBUS_BYTE_DATA;	//1个字节大小的数据
	args.data = &data;			//保存数据
	ret = ioctl(fd, I2C_SMBUS, &args);
	if (ret!=0) {
		perror("写入ioctl错误");
		return -1;
	}
	return 0;
}
signed short  adxl_read_short(int fd, unsigned char reg)
{
	union i2c_smbus_data data;
	struct i2c_smbus_ioctl_data args;
	int ret;
	args.read_write = I2C_SMBUS_READ;	//读
	args.command = reg;			//地址
	args.size = I2C_SMBUS_WORD_DATA;	//2字节大小的数据(short型)
	args.data = &data;			//保存数据
	ret = ioctl(fd, I2C_SMBUS, &args);
	if (ret!=0) {
		perror("读取short ioctl错误");
		return -1; 			//这个数值不妥,但是就先这样吧:)
	}
	return data.word;
}
```
#### 编译
```bash
arm-linux-gcc adxl345-simplest-use.c -o app -Wall
```
#### 运行
```
[root@FriendlyARM plg]# ./app 
器件ID[0x00]=0xe5
x=  -70 y=  214 z=  123
x=  -46 y=  274 z=   92
x=   21 y=  153 z= -154
x= -274 y=  209 z=  -20
x= -512 y=  189 z=  135
x= -512 y=  296 z=  415
x=    4 y= -194 z=  160
x=   40 y=  254 z=  210
x=  277 y= -160 z=  244
x=  498 y=   30 z= -181
x= -369 y=  -83 z=  -14
x= -167 y=   55 z=  -36
```
我承认我在乱晃XD.