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
user=bbb #vpn用户名
password-flags=1 #保存密码
require-mppe=yes #一些选项
require-mppe=yes
refuse-chap=yes
refuse-eap=yes
refuse-pap=yes


[ipv6]
method=auto

[ipv4]
method=auto
never-default=true #上文的:"仅将此连接用于相对应的网络上的资源"

```
# webbox/linux命令行

http://wiki.hidemyass.com/Tutorials:Connect_PPTP_on_Linux_command_line

Check if ppp-generic module exists. If not, it will probably not work:

    modprobe ppp-generic
 

Install necessary packages:

    apt-get install pptp-linux pptpd ppp curl
 
Create PPTP configuration file:

    nano /etc/ppp/peers/hmavpn


Enter this as content of the "hmavpn" file:

(replace 72.11.154.130 is the IP of the PPTP server you want to connect to, and MYHMAACCOUNTUSERNAME with your username)

```
pty "pptp 72.11.154.130 --nolaunchpppd"
lock
noauth
nobsdcomp
nodeflate
name MYHMAACCOUNTUSERNAME #用户名
remotename hmavpn #接口名
ipparam hmavpn #接口名
require-mppe-128  # 加密方式,debian默认是这样的,windosxp可以通过
usepeerdns # 使用vpnserver的dns,不上网可以不用
defaultroute # 没有设置也可以连上私有网络,反正不上网
persist # 自己加的,不知道
password 123456 # 直接这里写密码也是可以的
```

Enter VPN login credentials into chap-secrets file:
([tab] being replaced by a tab, username with your VPN account username and password with your PPTP password):

    nano /etc/ppp/chap-secrets 编辑连接用户
    username[tab]hmavpn[tab]password[tab]*

Create script to replace default routes - otherwise the VPN is not being used by your system:

    nano /etc/ppp/ip-up.local

Enter this as content of the "ip-up.local" file: 使用vpn的路由.如果不用可以不添加

```bash
#!/bin/bash
H=`ps aux | grep 'pppd pty' | grep -v grep | awk '{print $14}'`
DG=`route -n | grep UG | awk '{print $2}'`
DEV=`route -n | grep UG | awk '{print $8}'`
route add -host $H gw $DG dev $DEV
route del default $DEV
route add default dev ppp0
```

Make this script executable:

    chmod +x /etc/ppp/ip-up.local

To connect to the VPN:

    pon hmavpn

To disconnect from the VPN:

    poff hmavpn

Check your current IP:

    curl http://checkip.dyndns.org

# 客户端之间访问

## 1.服务端转发

http://superuser.com/questions/516634/pptp-linux-clients-unreachable

由vpnserver负责ip转发

ip转发 http://www.net-gyver.com/?p=1317

## 2.客户端路由表必须路由到vpn server

```
root@arm:/etc/ppp/peers# route add  10.0.0.0  gw 10.0.0.1 dev ppp0                                                          
root@arm:/etc/ppp/peers# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.2.253   0.0.0.0         UG    0      0        0 eth0
10.0.0.0        10.0.0.1        255.255.255.255 UGH   0      0        0 ppp0 # 路由转发给10.0.0.1,可以连接其他客户端
10.0.0.1        0.0.0.0         255.255.255.255 UH    0      0        0 ppp0 # 默认与服务器的连接
192.168.2.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0
192.168.2.100   0.0.0.0         255.255.255.255 UH    0      0        0 eth0
192.168.7.0     0.0.0.0         255.255.255.252 U     0      0        0 usb0
```
如果不使用vpn的路由(即不使用vpn上网)则客户端需要手动配置路由表,linux如上,window不知道.

为了简单起见,可以放弃"不使用vpn路由",这样就是不一定能上网.