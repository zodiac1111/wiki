# i2c驱动模型

根据最简单的i2c-stub.c分析

* 初始化:i2c_stub_init
* 退出:i2c_stub_exit
* struct i2c_algorithm 结构体
 * functionality 支持的功能
 * smbus_xfer 类似各种分支控制

* stub_adapter 适配器,在初始化时i2c_add_adapter添加 ,退出时由i2c_del_adapter移除
