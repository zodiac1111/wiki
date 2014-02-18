# modbus调试软件

modpoll modbus客户端

## 主站软件 

支持平台:

1. Windows
2. Linux (x86 32-bit)
3. Solaris (SPARC)
4. QNX Neutrino 6 (x86)

# 使用

## 作为modbus/TCP客户端

	modpoll -m tcp -a 2 -t4:hex -r 10 -c 3  127.0.0.1 -1 -p 10001

* -m tcp tcp模式,区分rtu模式
* -a 2 设备地址 2
* -t4:hex 数据类型
* -r 10 寄存器 10号
* -c 3  读取3个寄存器
* 127.0.0.1 IP
* -p 10001 端口

## 作为modbus/RTU客户端

    TODO


## 参考/来源:
1. 主站(master)测试端:[modpoll](http://www.modbusdriver.com/modpoll.html)
