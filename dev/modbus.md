# modbus的一些解释

modbus的误解 http://www.ccontrolsys.com/w/Common_Modbus_Protocol_Misconceptions

## 寄存器

寄存器,3/4开头的,其实是一个前导符号,只是为了区分不同区域,并不算在寄存器地址中.比如41001实际上是指1001寄存器.所以看到41001和465536这样的寄存器地址也不要奇怪.

协议寄存器实际上是从1开始的,虽然代码中是以0开始的.实际上1开始符合人类思维,而实现负责把base on 1 转化为base on 0.另外从站地址的确是以0开始的.

## 字节序

大小端

This is because when the Modbus standard was created in the late 1970's, most processors used a Big-Endian memory architecture.标准建立时大端机器

However when the need for transferring 32-bit (i.e. 4 byte) values with the Modbus protocol later came about in the 1980's, Little-Endian Intel processors dominated the PC market so most vendors chose to map the least significant word onto the lower address of the register pair.需要32位时小端机器