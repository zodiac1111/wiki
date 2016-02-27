
# sim800c at tcp 例子

## 单路 非透传

检查 SIM 卡状态

	AT+CPIN?
	+CPIN: READY
	OK

检查网络信号强度

	AT+CSQ
	CSQ: 20,0
	OK

检查网络注册状态
AT+CREG?
+CREG: 0,1
OK


检查 GPRS 附着状态

	AT+CGATT?
	+CGATT: 1
	OK

开始任务,设置 APN

	AT+CSTT="CMNET"
	OK

默认 APN 是 “CMNET”, 没有用户名
和密码。可以查询当地 GSM 运营商来
获得 APN


建立无线链路 (GPRS 或者 CSD)

	AT+CIICR
	OK

获得本地 IP 地址
AT+CIFSR
10.78.245.128

建立 TCP 链接

// AT+CIPSTART="TCP","116.228.221.51","8500"
AT+CIPSTART="TCP","bot9.us","8002"

OK
CONNECT OK //TCP 链接成功建立


AT+CIPSEND
> hello TCP serve

SEND OK









