# squid http代理

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