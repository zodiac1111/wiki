# lsof socket文件查看

> 引源:
> * http://blog.csdn.net/kozazyh/article/details/5495532
> *  linux lsof详解 http://blog.csdn.net/guoguo1980/article/details/2324454


lsof - list open files

# lsof命令是什么？

可以列出被进程所打开的文件的信息。被打开的文件可以是

1. 普通的文件
2. 目录  
3. 网络文件系统的文件，
4. 字符设备文件  
5. (函数)共享库  
6. 管道，命名管道 
7. 符号链接
8. 底层的socket字流，网络socket，unix域名socket
9. 在linux里面，大部分的东西都是被当做文件的…..还有其他很多

# 怎样使用lsof

这里主要用案例的形式来介绍lsof 命令的使用

## 列出所有打开的文件:

    lsof

备注: 如果不加任何参数，就会打开所有被打开的文件，建议加上一下参数来具体定位

##  查看谁正在使用某个文件

    lsof   /filepath/file

## 递归查看某个目录的文件信息

    lsof +D /filepath/filepath2/

备注: 使用了+D，对应目录下的所有子目录和文件都会被列出

## 比使用+D选项，遍历查看某个目录的所有文件信息 的方法

    lsof | grep ‘/filepath/filepath2/’

## 列出某个用户打开的文件信息

    lsof  -u username

备注: -u 选项，u其实是user的缩写

## 列出某个程序所打开的文件信息

    lsof -c mysql

备注: -c 选项将会列出所有以mysql开头的程序的文件，其实你也可以写成 lsof | grep mysql, 但是第一种方法明显比第二种方法要少打几个字符了

## 列出多个程序多打开的文件信息

    lsof -c mysql -c apache

## 列出某个用户以及某个程序所打开的文件信息

    lsof -u test -c mysql

## 列出除了某个用户外的被打开的文件信息

    lsof   -u ^root

备注：^这个符号在用户名之前，将会把是root用户打开的进程不让显示

## 通过某个进程号显示该进行打开的文件

    lsof -p 1

## 列出多个进程号对应的文件信息

    lsof -p 123,456,789

## 列出除了某个进程号，其他进程号所打开的文件信息

    lsof -p ^1

## 列出所有的网络连接

    lsof -i

## 列出所有tcp 网络连接信息

    lsof  -i tcp

## 列出所有udp网络连接信息

    lsof  -i udp

## 列出谁在使用某个端口

    lsof -i :3306

## 列出谁在使用某个特定的udp端口

    lsof -i udp:55

特定的tcp端口

    lsof -i tcp:80

## 列出某个用户的所有活跃的网络端口

    lsof  -a -u test -i

## 列出所有网络文件系统

    lsof -N

## 域名socket文件

    lsof -u

## 某个用户组所打开的文件信息

    lsof -g 5555

## 根据文件描述列出对应的文件信息

    lsof -d description(like 2)

## 根据文件描述范围列出文件信息

    lsof -d 2-3