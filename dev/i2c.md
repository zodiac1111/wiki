# i2c相关

## 设配规则

udev设配规则:https://xgoat.com/wp/2008/01/29/i2c-device-udev-rule/

```
KERNEL=="i2c-[0-9]*", GROUP="i2c"
```

## 一些参考 

I2C on a Linux based embedded system(pdf):  
http://home.deib.polimi.it/bellasi/lib/exe/fetch.php?media=students:giginghelli_finalreport.pdf

Behavioural analysis of an I2C Linux Driver(pdf) i2c linux 驱动行为分析:  
http://alexandria.tue.nl/repository/books/653250.pdf

I2C Drivers, Part I(2003年):  
http://www.linuxjournal.com/article/7136

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