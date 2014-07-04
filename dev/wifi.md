# wifi 类型

# wpa_cli 扫描

	wpa_cli -p/var/run/wpa_supplicant scan
	wpa_cli -p/var/run/wpa_supplicant scan_results

# 结果

	bssid / frequency / signal level / flags / ssid
	74:ea:3a:54:eb:2c       2442    53      [ESS]   webbox-wifi-test

flags理解

一些定义: http://www.cs.miami.edu/~burt/learning/Csc524.052/notes/wifi.html

Definitions:

* BSS
	Basic Service Set. A bunch of machines forming a cell.
* ESS
	Extended Service Set. Using WiFi beyond a BSS, gluing together several BSS
* BSSID
	A 48 bit identifier for a BSS. If an infrastructure BSS, it is the MAC of the 802.11 side of the Acess Point. Else the local bit is set and a 48-bit identifier is randomly selected.
* SSID
	Service set Identifier. An character string identifier for a ESS.
* NAV
	Network Access Vector. A time slot reservation, in microseconds.
* RTS/CTS
	Request To Send, Clear To Send. Reservation mechanism. Source,

WIFI 各种加密模式

1.AP
a: open 开放式 (没有加密选项)
74:ea:3a:54:eb:2c       2442    53      [ESS]   webbox-wifi-test

开启,自动选择, 16进制 ascii密码
74:ea:3a:54:eb:2c       2412    73      [ESS]   webbox-wifi-test

开放系统

共享密钥

	WPA/WPA2 安全选项:自动选择 加密方法:自动选择
	74:ea:3a:54:eb:2c       2412    84      [WPA-EAP-TKIP+CCMP][WPA2-EAP-TKIP+CCMP-preauth][ESS]    webbox-wifi-test

	WPA/WPA2 安全选项:WPA 加密方法:自动选择
	74:ea:3a:54:eb:2c       2462    84      [WPA-EAP-TKIP+CCMP][ESS]        webbox-wifi-test

	WPA/WPA2 安全选项:WPA 加密方法:TKIP
	74:ea:3a:54:eb:2c       2462    76      [WPA-EAP-TKIP][ESS]     webbox-wifi-test

	WPA/WPA2 安全选项:WPA 加密方法:AES
	74:ea:3a:54:eb:2c       2437    74      [WPA-EAP-CCMP][ESS]     webbox-wifi-test

	WPA/WPA2 安全选项:WPA2 加密方法:自动选择
	74:ea:3a:54:eb:2c       2412    66      [WPA2-EAP-TKIP+CCMP-preauth][ESS]       webbox-wifi-test

	WPA/WPA2 安全选项:WPA2 加密方法:TKIP
	74:ea:3a:54:eb:2c       2412    90      [WPA2-EAP-TKIP-preauth][ESS]    webbox-wifi-test

	WPA/WPA2 安全选项:WPA2 加密方法:AES
	74:ea:3a:54:eb:2c       2462    81      [WPA2-EAP-CCMP-preauth][ESS]    webbox-wifi-test

	WPA-PSK/WPA2-PSK 安全选项:自动选择 加密方法:自动选择
	74:ea:3a:54:eb:2c       2412    92      [WPA-PSK-TKIP+CCMP][WPA2-PSK-TKIP+CCMP-preauth][ESS]    webbox-wifi-tes

	WPA-PSK/WPA2-PSK 安全选项:WPA-PSK 加密方法:TKIP
	74:ea:3a:54:eb:2c       2462    78      [WPA-PSK-TKIP][ESS]     webbox-wifi-test

	WPA-PSK/WPA2-PSK 安全选项:WPA-PSK 加密方法:AES
	74:ea:3a:54:eb:2c       2462    78      [WPA-PSK-CCMP][ESS]     webbox-wifi-test

	WPA-PSK/WPA2-PSK 安全选项:WPA2-PSK 加密方法:AES
	74:ea:3a:54:eb:2c       2462    78      [WPA2-PSK-CCMP-preauth][ESS]    webbox-wifi-test

	ad_hoc Wireless ad hoc network无线临时网络 (暂时不考虑)

# 有用的指令

	#  查询状态
	wpa_cli -p/var/run/wpa_supplicant status

无效

	wpa_state=INACTIVE

	# 查看网络
	wpa_cli -p/var/run/wpa_supplicant list_networks
	Selected interface 'wlan0'
	network id / ssid / bssid / flags
	0       webbox-wifi-test        any     [DISABLED]

选择使能一个,同时使其他无效,从list_networks中选择,使能后就连接了
	
	wpa_cli -p/var/run/wpa_supplicant select_network 0

再查看一下

	[root@AT91SAM9-RT9x5 bin]# wpa_cli -p/var/run/wpa_supplicant status
	Selected interface 'wlan0'
	bssid=74:ea:3a:54:eb:2c
	ssid=webbox-wifi-test
	id=0
	mode=station
	pairwise_cipher=CCMP
	group_cipher=CCMP
	key_mgmt=WPA-PSK
	wpa_state=COMPLETED


