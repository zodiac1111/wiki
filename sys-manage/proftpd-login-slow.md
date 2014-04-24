# ftp登陆缓慢

因为没有dns,ftp先查找dns.所以等待很久.

表现为登陆非常慢,可能等待一个固定的值,12/15/30/60秒等. 登陆之后操作完全没问题.

* proftpd 服务器: 配置文件加 `UseReverseDNS off`
* vsftpd: 配置文件 .conf 加 `reverse_lookup_enable=NO`

参考:
* [proftpd](http://trinityhome.org/Home/index.php?content=SLOW_RESPONSE_ON_CONNECTING_TO_PROFTPD_SERVER__SLO&front_id=18&lang=en&locale=en)
* [vsftpd](http://serverfault.com/questions/406437/slow-ftp-issue-when-there-is-no-dns-server)