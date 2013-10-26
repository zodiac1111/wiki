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