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
* webbox(command line/配置文件)

