# AT指令

at指令既[海斯命令集](https://en.wikipedia.org/wiki/Hayes_command_set).

# 常用例子

可能与具体模块实现有关.待分离.

```
AT+CSQ # 查询信号强度

# +CIMI:查询模块的国际移动台标号
AT+CIMI

# 设定基站id格式并获得基站id(mc2761_老版本没有)
AT+CREG=2
AT+CREG?

# 厂商标识
AT+CGMI
```
