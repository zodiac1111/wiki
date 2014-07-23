微型的dhcp软件{#udhcp}
=======================

现在只用他的服务器功能,这样电脑直接插eth0 选择自动获取就可以得到ip了.

然后登陆192.168.1.2就可以了,类似路由器.

## 编译

	$ make CROSS_COMPILE=arm-linux-

结果

	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer dhcpd.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer arpping.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer files.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer leases.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer serverpacket.c
	serverpacket.c: 在函数‘add_bootp_options’中:
	serverpacket.c:101: 警告：传递‘strncpy’的参数 1 给指针时，目标与指针符号不一致
	serverpacket.c:103: 警告：传递‘strncpy’的参数 1 给指针时，目标与指针符号不一致
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer options.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer socket.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer packet.c
	packet.c: 在函数‘get_packet’中:
	packet.c:73: 警告：传递‘strncmp’的参数 1 给指针时，目标与指针符号不一致
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer pidfile.c
	arm-linux-gcc  dhcpd.o arpping.o files.o leases.o serverpacket.o options.o socket.o packet.o pidfile.o -o udhcpd
	files.o: In function `read_ip':
	files.c:(.text+0x5e4): warning: gethostbyname is obsolescent, use getnameinfo() instead.
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer dhcpc.c
	dhcpc.c: 在函数‘perform_renew’中:
	dhcpc.c:134: 错误：标号位于复合语句末尾
	dhcpc.c: 在函数‘main’中:
	dhcpc.c:254: 警告：传递‘strncpy’的参数 1 给指针时，目标与指针符号不一致
	dhcpc.c:269: 警告：传递‘strncpy’的参数 1 给指针时，目标与指针符号不一致
	Makefile:71: recipe for target 'dhcpc.o' failed
	make: *** [dhcpc.o] Error 1

修改那个语句,`case`后面加个`break;`就行了

## 在继续编译

	zodiac1111@debian:udhcp-0.9.8$ make CROSS_COMPILE=arm-linux-
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer dhcpc.c
	dhcpc.c: 在函数‘main’中:
	dhcpc.c:255: 警告：传递‘strncpy’的参数 1 给指针时，目标与指针符号不一致
	dhcpc.c:270: 警告：传递‘strncpy’的参数 1 给指针时，目标与指针符号不一致
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer clientpacket.c
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer script.c
	script.c: 在函数‘fill_envp’中:
	script.c:187: 警告：传递‘strlen’的参数 1 给指针时，目标与指针符号不一致
	script.c:193: 警告：传递‘strlen’的参数 1 给指针时，目标与指针符号不一致
	arm-linux-gcc  dhcpc.o clientpacket.o script.o options.o socket.o packet.o pidfile.o -o udhcpc
	arm-linux-gcc -c -DSYSLOG -W -Wall -Wstrict-prototypes -DVERSION='"0.9.8"' -Os -fomit-frame-pointer dumpleases.c
	arm-linux-gcc  dumpleases.o -o dumpleases
	arm-linux-strip --remove-section=.note --remove-section=.comment udhcpd udhcpc dumpleases

`/home/zodiac1111/Downloads/udhcp-0.9.8/udhcpd` 这个就是dhcp服务器了

`/home/zodiac1111/Downloads/udhcp-0.9.8/samples/udhcpd.conf` 是配置文件例子.

可根据`udhcp`的`readme`文件 使用 

	udhcpd <配置文件> 

指令指定多个配置文件,从而指定多个端口dhcp

按需要修改(1口的自动分配)

	start 		192.168.1.20	#default: 192.168.0.20
	end		192.168.1.254	#default: 192.168.0.254
