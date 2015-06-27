# 配置dns协议

加密dns,换tcp协议(默认是udp),换端口,防止

原始repo

https://github.com/jedisct1/dnscrypt-proxy

forked repo

https://github.com/opendns/dnscrypt-proxy/

	# dnscrypt-proxy -T

	/etc/resolv.conf 
	nameserver 127.0.0.1

dns测试

	dig @8.8.8.8 github
	dig @<DNS服务器> <检测的网站>
