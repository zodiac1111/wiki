# squid http代理

配置文件: `/etc/squid/squid.conf`

启动服务: `service squid restart`


匿名设置:

* Squid 2.x:
```
header_access Via deny all
header_access X-Forwarded-For deny all
```
* Squid 3.0
```
reply_header_access Via deny all
reply_header_access X-Forwarded-For deny all
```
* Squid 3.1
```
via off
forwarded_for delete
```