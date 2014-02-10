# 交叉编译gdbserver

未完成

> * http://www.latelee.org/using-gnu-linux/94-cross-compile-gdb-and-gdbserver.html
> * http://blog.csdn.net/subfate/article/details/6119477
> * `gdb-7.6/gdb/gdbserver/README`

## 下载gdb

下载地址为：http://ftp.gnu.org/gnu/gdb/

按照一般的想法，最新版本越好，因此下载7.2这个版本。当然，凡事无绝对。

我们以gdb-7.2.tar.bz2 这个文件为例。

## 解压缩

    $ tar jxvf gdb-7.2.tar.bz2

注：小技巧：Linux下一般压缩文件后缀为.tar.bz2和.tar.gz，它们解压命令有两三个选项是一致的：
xf（v），前者再加上j选项，后者再加上z选项。

## 进入该目录

    $ cd gdb-7.2/

## 配置

    $./configure --target=arm-linux --program-prefix=arm-linux- --prefix=/home/gotohell/gdb-build

注：
* `--target=arm-linux`意思是说目标平台是运行于ARM体系结构的linux内核；
* `--program-prefix=arm-linux-`是指生成的可执行文件的前缀，比如arm-linux-gdb，
* `--prefix`是指生成的可执行文件安装在哪个目录，这个目录需要根据实际情况作选择。如果该目录不存在，会自动创建，当然，权限足够的话。必须是绝对路径

## 编译、安装

    $ make

可能的问题:
```
In file included from gdb.c:19:0:
defs.h:105:17: fatal error: bfd.h: 没有那个文件或目录
```
安装这个包(debian)`binutils-dev`可以解决

```
In file included from ../libdecnumber/decNumber.h:37:0,
                 from ../libdecnumber/dpd/decimal128.h:58,
                 from dfp.c:29:
../libdecnumber/decContext.h:54:61: fatal error: gstdint.h: 没有那个文件或目录

```



    $ make install

# 使用

> * http://www.cnblogs.com/papam/archive/2009/11/20/1606873.html
> * http://stackoverflow.com/questions/9245685/gdb-no-symbol-table-is-loaded

目标机程序`-g`编译

## 目标机(target)

目标机运行gdbserver监听程序

    gdbserver 127.0.0.1:3456 app

* 127.0.0.1 要监听的ip(本机ip)
* 3456监听端口,host就重这的端口连到terget上
* app 运行的程序

## 主机(host)

host运行arm-linux-gdb

  arm-linux-gdb
  (gdb) file app (加载符号表,不懂)

### 实例
```
zodiac1111@debian:src_linux$ arm-linux-gdb
GNU gdb 6.8
Copyright (C) 2008 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "--host=i686-pc-linux-gnu --target=arm-unknown-linux-uclibcgnueabi".
(gdb) target remote 192.168.1.2:3456
Remote debugging using 192.168.1.2:3456
[New Thread 859]
0x40021910 in ?? ()
(gdb) file netcomm
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
netcomm: No such file or directory.
(gdb) file netcomm/netcomm
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
Reading symbols from /home/zodiac1111/workspace/webbox/01-Trunk/01-Code/src_linux/netcomm/netcomm...done.
(gdb) l
12	//
13	#include "center.h"
14	#include "station.h"
15	#include "netcomm.h"
16	
17	
18	int dc_do(CDataCenter dc);
19	int main(void)
20	{
21		clog_start();
(gdb) b main
Breakpoint 1 at 0x23874: file netcomm.c, line 21.
(gdb) c
Continuing.

Breakpoint 1, main () at netcomm.c:21
21		clog_start();
(gdb) n
28		if (initStationData()<0) {
(gdb) s
initStationData () at ../netcomm/station.c:37
37		st = loadStation();

```
