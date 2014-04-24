# ftp/ssh登陆缓慢

# ftp登陆缓慢

## 现象
因为没有dns,ftp先查找dns.所以等待很久.

表现为登陆非常慢,可能等待一个固定的值,12/15/30/60秒等. 登陆之后操作完全没问题.

## 解决方式

## 修改不要查找dns服务器

这样就应该没法使用域名了

* proftpd 服务器: 配置文件加 `UseReverseDNS off`
* vsftpd: 配置文件 .conf 加 `reverse_lookup_enable=NO`

## 开始dns服务

`dns -d`可能还要跟`/etc/resolv.conf`域名解析配合

## 参考
* [proftpd](http://trinityhome.org/Home/index.php?content=SLOW_RESPONSE_ON_CONNECTING_TO_PROFTPD_SERVER__SLO&front_id=18&lang=en&locale=en)
* [vsftpd](http://serverfault.com/questions/406437/slow-ftp-issue-when-there-is-no-dns-server)

# ssh登陆缓慢,也是dns惹的祸

## 解决方式

openSSH

* add "UseDNS no" to /etc/ssh/sshd_config
* add the client's net address to the server's /etc/hosts

dropbear

* edit options.h and comment out where DO_HOST_LOOKUP is defined -
* recompile.
 
## 参考
*  http://www.turnkeylinux.org/blog/slow-ssh
* http://forum.stmlabs.com/showthread.php?tid=10683