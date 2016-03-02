
# sim800c at tcp 例子

# 单链路模式

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
	或者
	AT+CIPSTART="TCP","192.227.251.64","8002"
	OK
	CONNECT OK //TCP 链接成功建立

发送数据到远端服务, CTRL+Z (0x1a)

	AT+CIPSEND
	> hello TCP serve

	SEND OK

(主动关闭)关闭 TCP 连接

	AT+CIPCLOSE
	CLOSE OK

## 透传模式

检查 GPRS 附着状态

	AT+CGATT?
	+CGATT: 1
	OK

设置链接模式为透传模式

	AT+CIPMODE=1
	OK

开始任务,设置 APN,参考注释 [1]

	AT+CSTT=”CMNET”
	OK

建立无线链路 (GPRS 或者 CSD)

	AT+CIICR
	OK

获得本地 IP 地址

	AT+CIFSR
	10.78.245.128
	AT+CIPSTART=”TCP”,”116.228.221.51”,”8500”
	OK
	CONNECT 建立 TCP 链路成功建立链接,进入数据模式
	OK // 通过拉高 DTR 或者 “+++”退出数据模式

	ATO // 重新切回到数据模式

# 多链路模式

检查 GPRS 附着状态

	AT+CGATT?
	+CGATT: 1 检查 GPRS 附着状态
	OK

设置多链路模式

	AT+CIPMUX=1
	OK

开始任务,设置 APN
	AT+CSTT="CMNET"
	OK

建立无线链路(GPRS 或者 CSD)

	AT+CIICR
	OK

获得本地 IP 地址
	AT+CIFSR
	10.78.245.128

在第 0 路建立 TCP 链接

	AT+CIPSTART=0,"TCP","bot9.us","8002"
	OK
	0, CONNECT OK

在第 1 路建立 TCP 链接

	AT+CIPSTART=1,"TCP","192.227.251.64","8002"
	OK
	1, CONNECT OK

第0路发送数据

	AT+CIPSEND=0
	> TCP test
	0, SEND OK

第1路发送数据

	AT+CIPSEND=1
	> TCP1 test
	0, SEND OK

查询当前链接状态
	AT+CIPSTATUS
	OK

	STATE: IP PROCESSING

	C: 0,0,"TCP","23.95.25.247","8002","CONNECTED"
	C: 1,0,"TCP","192.227.251.64","8002","CLOSED"
	C: 2,,"","","","INITIAL"
	C: 3,,"","","","INITIAL"
	C: 4,,"","","","INITIAL"
	C: 5,,"","","","INITIAL"


新塘
SendChar_ToUART 问题 NuTiny

lwip?ethernet
















