# 网络相关

# ifconfig
修改ip和子网掩码   执行这个命令：

```
ifconfig   eth0   10.10.10.131   netmask   255.255.255.0
或者
ifconfig   eth0:0   192.168.1.103   netmask   255.255.255.0
```

网关的设定执行这个命令:
```
route   add   default   gw   10.10.10.6
```

把这两个命令写到/etc/rc.local   或者/etc/rc.d/rc.local   里面都可以的，这样就可以永久保存


我的备份指令(路由)

	ifconfig   eth0  192.168.1.103   netmask   255.255.255.0
 	route   add   default   gw   192.168.1.253

如果执行了上面的命令后，还不可以的话，可能就是你的网卡没有激活， 
你可以用ifconfig   eth0   查看你的网卡信息，如果没有激活的话就用ifconfig   eth0   up,或者ifup   eth0   ,要停止的话，就用ifconfig   eth0   down   或者ifdown   eth0

 
	ifconfig   eth0   192.168.1.168   netmask   255.255.255.0
	route   add   default   gw   192.168.1.253

	ifconfig   eth0   10.10.10.131   netmask   255.255.255.0
	route   add   default   gw   10.10.10.6
	insmod /var/ftp/motor.ko &
	/var/ftp/motor &

	ifconfig em1:1 192.168.2.212 #em1是fedora新的对内建网卡设配命名.传统是eth0,1,..这样

# network/interfaces

所有网络接口

一个简单例子

```
# 基本的回环接口
# Configure Loopback 
auto lo
iface lo inet loopback

# 基本的eth0 有线接口
# The primary network interface
auto eth0
iface eth0 inet static
address 192.168.1.2
gateway 192.168.1.1
netmask 255.255.255.0
hwaddress ether 00:21:C8:17:1C:00
#network 192.168.1.0
#broadcast 192.168.1.255
```