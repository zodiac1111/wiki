参考

* [Command for determining my public IP?](http://askubuntu.com/questions/95910/command-for-determining-my-public-ip)
* [Windows command that returns external IP](http://superuser.com/questions/165986/windows-command-that-returns-external-ip)
* [How can I get my external IP address in bash?](http://unix.stackexchange.com/questions/22615/how-can-i-get-my-external-ip-address-in-bash)

# 获得IP


myexternalip.com

	curl "http://myexternalip.com/raw"

checkip.dyndns.org

	curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'

ipecho.net

	wget -qO- http://ipecho.net/plain ; echo

ifconfig.me

	curl ifconfig.me

icanhazip.com

	wget -qO - icanhazip.com
	curl icanhazip.com
	curl ipv4.icanhazip.com
	curl ipv6.icanhazip.com

ident.me

	curl ident.me

opendns.com

	dig +short myip.opendns.com @resolver1.opendns.com

ifcfg.me

	telnet 4.ifcfg.me 2>&1 | grep IPv4 | cut -d' ' -f4

[更多提供ip服务链接](http://unix.stackexchange.com/a/128088/23140)


# 定位

https://www.iplocation.net/

	curl ipinfo.io/23.66.166.151


	curl ipinfo.io

