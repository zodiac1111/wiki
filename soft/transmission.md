# transmission bt客户端 

需要图形界面依赖.

>  参考 https://wiki.archlinux.org/index.php/Transmission_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

transmission-cli

包括命令行工具、守护进程和 web 客户端.

# 安装

> http://pkgs.org/centos-6/epel-i386/transmission-daemon-2.13-1.el6.i686.rpm.html

    yum install transmission-daemon

# 配置

配置文件` ~/.config/transmission-daemon/settings.json`
默认端口 :9091
默认白名单:本机 127.0.0.1