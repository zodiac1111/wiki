# transmission bt客户端 

需要图形界面依赖.

>  参考 https://wiki.archlinux.org/index.php/Transmission_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

transmission-cli

包括命令行工具、守护进程和 web 客户端.

# 安装

> http://pkgs.org/centos-6/epel-i386/transmission-daemon-2.13-1.el6.i686.rpm.html

    yum install transmission-daemon

# 配置

必须停掉 transmission-daemon 才能修改配置文件 

配置文件` ~/.config/transmission-daemon/settings.json`

默认端口 :9091

默认白名单:本机 127.0.0.1 rpc-whitelist

命令行方式指定白名单 

  transmission-daemon --allowed 115.198.91.197(自己的IP)

可以指定多个白名单 "rpc-whitelist": "127.0.0.1,192.168.*.*"  [参见这里](https://trac.transmissionbt.com/wiki/EditConfigFiles)

使用用户名密码登陆 [参见这里](http://www.hdpfans.com/thread-11614-1-1.html)

```
"rpc-authentication-required": true,
"rpc-enabled": true,
"rpc-password": "密码",
"rpc-username": "root",
```
```
~ # transmission-deamon --help
-t   --auth                             Require authentication
-T   --no-auth                          Don't require authentication
```