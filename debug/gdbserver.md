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