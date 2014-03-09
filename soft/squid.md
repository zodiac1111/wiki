# squid http代理

# 安装

## debian

``
apt-get install squid
``

# 配置
中文翻译(Squid中文权威指南 2005年) http://home.arcor.de/pangj/squid/index.html

配置文件: `/etc/squid/squid.conf`

启动服务: `service squid restart`

默认端口 3128

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

# 开机启动

查看启动项
```
chkconfig
```

启动之
```
chkconfig squid on
```