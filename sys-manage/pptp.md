# pptp 客户端设定

## 客户端不使用vpn访问互联网

不替换默认路由,vpn仅用于对应网络的连接.依然用过本机直接访问互联网,而不是通过vpn访问.

路由表类似:

```text
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.2.253   0.0.0.0         UG    1024   0        0 wlan0 #原始的不变
10.0.0.1        0.0.0.0         255.255.255.255 UH    0      0        0 ppp1 #vpn作用于对应网络
10.0.0.2        0.0.0.0         255.255.255.255 UH    0      0        0 ppp0 #(这里服务器打通所有客户端)
10.0.0.2        0.0.0.0         255.255.255.255 UH    0      0        0 ppp2 #
169.254.0.0     0.0.0.0         255.255.0.0     U     1000   0        0 wlan0
192.168.2.0     0.0.0.0         255.255.255.0   U     0      0        0 wlan0
```

* windowXP 不代替默认路由(连上vpn还可以通过本机访问互联网,而不是通过vpn访问互联网) [这里](http://service.tp-link.com.cn/detail_article_414.html)

linux

* debian/mate/gnome(gui): 编辑连接->IPv4设置->路由->勾选"仅将此连接用于相对应的网络上的资源"
* webbox(command line/配置文件) 配置文件见下面

# debian network

gui的配置文件`sudo cat '/etc/NetworkManager/system-connections/本机测试' `

```text
[connection]
id=本机测试 # gui上显示的名称
uuid=428ad377-aab9-4d95-9e9c-xxxxxxxxx
type=vpn
permissions=user:zodiac1111:;
autoconnect=false
timestamp=1408782468

[vpn]
service-type=org.freedesktop.NetworkManager.pptp
gateway=192.168.2.100 # vpn 主机
require-mppe=yes #一些选项
user=bbb #vpn用户名
password-flags=1
require-mppe=yes
user=bbb
refuse-chap=yes
refuse-eap=yes
password-flags=1
refuse-pap=yes


[ipv6]
method=auto

[ipv4]
method=auto
never-default=true #上文的:"仅将此连接用于相对应的网络上的资源"

```