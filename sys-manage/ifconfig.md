# 网络相关

# ifconfig

修改ip和子网掩码   执行这个命令：


	ifconfig   eth0   10.10.10.131   netmask   255.255.255.0
	或者
	ifconfig   eth0:0   192.168.1.103   netmask   255.255.255.0

删除指定路由

	route del -net 192.168.2.0 gw 192.168.1.1 netmask 255.255.255.0 dev eth1

网关的设定执行这个命令:

	route   add   default   gw   10.10.10.6

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

> 参考:
> * http://man.chinaunix.net/linux/debian/debian_learning/ch11s12.html
> * http://www.cyberciti.biz/faq/setting-up-an-network-interfaces-file/ 英文,详尽
> * http://gfrog.net/2008/01/config-file-in-debian-interfaces-1/ 中文,详细,讲解interfaces配置
> * `man interfaces` 英文,详细,权威

配合的指令: `ifup(8)` `ifdown(8)`

### 例子:官方示例文件

[[interfaces]]

### 例子:回环接口

	# 基本的回环接口
	# Configure Loopback 
	auto lo
	iface lo inet loopback

### 例子:静态有线

	# 基本的eth0 有线接口
	# The primary network interface
	# (network broadcast 和 gateway 等 项是可选的)
	auto eth0 # 自动,名称
	iface eth0 inet static # 接口 名称 因特网 静态(动态 dhcp)
	address 192.168.1.2 # ip地址
	gateway 192.168.1.1 # 网关(可选)
	netmask 255.255.255.0 # 子网掩码
	hwaddress ether 00:21:C8:17:1C:00 # MAC地址
	name em1 #(可选)
	#network 192.168.1.0 # (可选)
	#broadcast 192.168.1.255 #广播地址 (可选)
	dns-nameservers 1.1.1.1 #dns服务器 (可选)
	dns-search .com #(可选)

### 例子:动态有线
	# The first network card - this entry was created during the Debian installation
	# (network, broadcast and gateway are optional)
	auto eth0
	iface eth0 inet dhcp
### 例子:虚拟有线
	# 设定IP地址(虚拟IP地址)####
	auto eth0:1 # :X 虚拟网卡
	iface eth0:1 inet static
	address x.x.x.x
	netmask x.x.x.x
	network x.x.x.x
	broadcast x.x.x.x
	gateway x.x.x.x

# route

> 鸟哥私房菜 http://linux.vbird.org/linux_server/0140networkcommand.php#route

添加默认路由

	route add -net 0.0.0.0 netmask 0.0.0.0 gateway 192.168.2.1 dev eth0

# windows xp 路由操作

打印路由表
    route print