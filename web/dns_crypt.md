# 配置dns协议

加密dns/自定义dns服务,一定程度防止dns劫持

加密dns,换tcp协议(默认是udp),换端口,防止劫持

原始repo

https://github.com/jedisct1/dnscrypt-proxy

forked repo

https://github.com/opendns/dnscrypt-proxy/

debian包: https://packages.debian.org/sid/amd64/dnscrypt-proxy/download

使用

	# dnscrypt-proxy -T

	/etc/resolv.conf 
	nameserver 127.0.0.1

以上貌似过期,

参考[这里](https://github.com/jedisct1/dnscrypt-proxy/blob/master/dnscrypt-resolvers.csv)

    dnscrypt-proxy -R <名字>


dns测试

	dig @8.8.8.8 github
	dig @<DNS服务器> <检测的网站>
