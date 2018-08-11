# iocage

iocage: A FreeBSD jail manager written in Python 3

> The jails infrastructure is transitioning from the old warden backend to the new iocage backend.

官方网址 [[https://github.com/freenas/iocage-ix-plugins]]

## 参考

* jail [[https://doc.freenas.org/11/jails.html]]


## 常用命令

官方教程 [[https://doc.freenas.org/11/jails.html#using-iocage]]

基本使用教程: [[http://iocage.readthedocs.io/en/latest/basic-use.html]]

通用/一般操作

```
# 列出jails
iocage list

# 设置激活的zpool
iocage activate <zpool>

# 显示帮助信息
iocage --help | more

# 创建jail
iocage create -n examplejail ip4_addr="em0|192.168.1.10/24" -r 11.1-RELEASE

# 启动jail
iocage start examplejail

# 进入jail的控制台
iocage console examplejail

# 停止
iocage stop examplejail

# 显示jail/虚拟机配置信息
iocage get all examplejail | less

```

类似git,抓取插件

```
iocage fetch --plugins --name "jenkins" ip4_addr="igb0|192.168.0.91/24"
```