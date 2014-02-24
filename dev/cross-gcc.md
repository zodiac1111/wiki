#集成构建工具
1. Buildroot
2. crosstool
#简明构建交叉编译器指导提示(gcc)
[来源](http://www.ifp.illinois.edu/~nakazato/tips/xgcc.html#pre)

# 交叉编译器命名规则

## 参考
* http://www.crifan.com/files/doc/docbook/cross_compile/release/webhelp/crosscompiler_naming_rule.html

## 一些名称例子

```
arm-xscale-linux-gnueabi-gcc
arm-liunx-gnu-gcc
arm-unknown-linux-uclibcgnueabi
```
定义: 
```
arch-vendor-kernel-system
<架构>-<供应商>-<内核>-<系统>
```
### 分析
#### arch
即系统架构.表示交叉编译器，是用于哪个目标系统架构中，用于那个平台中的

即，用此交叉编译器**编译出来的程序**，是**运行在哪种CPU**上面的

arch的值，常见的有很多种，比如arm，x86，mips等等。
#### kernel

你用此交叉编译器，编译出来的程序，所运行的目标系统

即，此交叉编译器，编译出来的程序，在什么系统中，什么环境中，运行。

Linux uclinux none

而对应的环境或系统，主要有两种：

* Linux
  表示：有OS（此处主要指的是Linux）操作系统的环境

  比如，我用交叉编译器，编译一个helloworld程序，然后下载到嵌入式开发中的嵌入式Linux中运行，

  就属于，用此交叉编译器，编译出来的程序，是要运行于，带OS，即嵌入式Linux系统，环境中的

  此处，简称为，有OS的目标系统：Linux

* bare-metal
  bare-metal，直译为：裸金属

  表示：无（此处主要指的是Linux）操作系统的环境，

  比如，用此交叉编译器，去编译一个Uboot，或者是其他一个小程序，是运行在，无嵌入式Linux的时候，单独运行的一个程序。

  比如，你购买的嵌入式系统开发版，常常附带一些小程序，比如点亮LED，跑马灯等程序，就是这种，运行在无OS的环境的

  此处，简称为：无OS系统的：bare-metal

#### system

直译为，系统

其实主要表示的，交叉编译器所选择的库函数和目标系统

最常见的一些值有，gnu，gnueabi，uclibcgnueabi等等

对应关系`oabi < - >eabi` http://www.crifan.com/order_eabi_and_oabi/ `readelf -h`

`glibc,eglibc,uclibc` http://www.crifan.com/relation_between_uclibc_glibc_eglibc/
#### 其他见参考


## 一个例子
`arm-buildroot-linux-uclibcgnueabi-gcc`
```
root@beaglebone:/usr/bin# file arm-angstrom-linux-gnueabi-gcc   
arm-angstrom-linux-gnueabi-gcc: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.16, stripped
```

#构建GCC交叉编译器
[来源](http://wiki.osdev.org/GCC_Cross-Compiler)
##1.gcc
Now, you can build GCC.

	cd /usr/src/build-gcc
	export PATH=$PATH:$PREFIX/bin
	../gcc-x.x.x/configure --target=$TARGET \ 
	--prefix=$PREFIX  \
	--disable-nls  \
	--enable-languages=c,c++ \
	--without-headers \

	make all-gcc
	make install-gcc
	* --disable-nls is the same as for binutils above.
	* --without-headers tells GCC not to rely on any C library (standard or runtime) being present for the target.
	* --enable-languages tells GCC not to compile all the other language frontends it supports, but only C (and optionally C++).
其他参数
arm-linux arm-elf 区别,C语言库区别.
[来源](http://hi.baidu.com/ajoe/blog/item/1780d100a2270a12728b6582.html)
##2.Clibrary
##3.Full Cross-Compiler整个编译器
##4.本地编译器
运行在arm上,且编译出运行在arm平台的可执行文件的gcc for arm 而不是 gcc for host build target for arm.

im-chooser输入法选择器
```
[lee@localhost gtktest1]$ arm-linux-gcc -v
Using built-in specs.
Target: arm-unknown-linux-uclibcgnueabi
Configured with: /home/zzl/buildroot-2011.02/output/toolchain/gcc-4.3.5/configure --prefix=/home/zzl/buildroot-2011.02/output/host/usr --build=i686-pc-linux-gnu --host=i686-pc-linux-gnu --target=arm-unknown-linux-uclibcgnueabi --enable-languages=c,c++ --with-sysroot=/home/zzl/buildroot-2011.02/output/host/usr/arm-unknown-linux-uclibcgnueabi/sysroot --with-build-time-tools=/home/zzl/buildroot-2011.02/output/host/usr/arm-unknown-linux-uclibcgnueabi/bin --disable-__cxa_atexit --enable-target-optspace --with-gnu-ld --disable-libssp --disable-multilib --disable-tls --enable-shared --with-gmp=/home/zzl/buildroot-2011.02/output/host/usr --with-mpfr=/home/zzl/buildroot-2011.02/output/host/usr --disable-nls --enable-threads --disable-decimal-float --with-float=soft --with-abi=aapcs-linux --with-arch=armv4t --with-tune=arm920t --disable-largefile --with-pkgversion='Buildroot 2011.02' --with-bugurl=http://bugs.buildroot.net/ : (reconfigured) /home/zzl/buildroot-2011.02/output/toolchain/gcc-4.3.5/configure --prefix=/home/zzl/buildroot-2011.02/output/host/usr --build=i686-pc-linux-gnu --host=i686-pc-linux-gnu --target=arm-unknown-linux-uclibcgnueabi --enable-languages=c,c++ --with-sysroot=/home/zzl/buildroot-2011.02/output/host/usr/arm-unknown-linux-uclibcgnueabi/sysroot --with-build-time-tools=/home/zzl/buildroot-2011.02/output/host/usr/arm-unknown-linux-uclibcgnueabi/bin --disable-__cxa_atexit --enable-target-optspace --with-gnu-ld --disable-libssp --disable-multilib --disable-tls --enable-shared --with-gmp=/home/zzl/buildroot-2011.02/output/host/usr --with-mpfr=/home/zzl/buildroot-2011.02/output/host/usr --disable-nls --enable-threads --disable-decimal-float --with-float=soft --with-abi=aapcs-linux --with-arch=armv4t --with-tune=arm920t --disable-largefile --with-pkgversion='Buildroot 2011.02' --with-bugurl=http://bugs.buildroot.net/
Thread model: posix
gcc version 4.3.5 (Buildroot 2011.02) 
```

```
#
'/home/zodiac1111/arm/buildroot/output/host/usr/bin/arm-linux-gcc' -v
Using built-in specs.
COLLECT_GCC=/home/zodiac1111/arm/buildroot/output/host/usr/bin/arm-linux-gcc
COLLECT_LTO_WRAPPER=/home/zodiac1111/arm/buildroot/output/host/usr/libexec/gcc/arm-unknown-linux-uclibcgnueabi/4.6.3/lto-wrapper
Target: arm-unknown-linux-uclibcgnueabi
Configured with: /home/zodiac1111/arm/buildroot/output/toolchain/gcc-4.6.3/configure --prefix=/home/zodiac1111/arm/buildroot/output/host/usr --build=i686-pc-linux-gnu --host=i686-pc-linux-gnu --target=arm-unknown-linux-uclibcgnueabi --enable-languages=c --with-sysroot=/home/zodiac1111/arm/buildroot/output/host/usr/arm-unknown-linux-uclibcgnueabi/sysroot --with-build-time-tools=/home/zodiac1111/arm/buildroot/output/host/usr/arm-unknown-linux-uclibcgnueabi/bin --disable-__cxa_atexit --enable-target-optspace --disable-libquadmath --disable-libgomp --with-gnu-ld --disable-libssp --disable-multilib --enable-tls --enable-shared --with-gmp=/home/zodiac1111/arm/buildroot/output/host/usr --with-mpfr=/home/zodiac1111/arm/buildroot/output/host/usr --with-mpc=/home/zodiac1111/arm/buildroot/output/host/usr --disable-nls --enable-threads --disable-decimal-float --with-float=soft --with-abi=aapcs-linux --with-arch=armv4t --with-tune=arm922t --with-pkgversion='Buildroot 2012.11-git-00344-g2410e3d-dirty' --with-bugurl=http://bugs.buildroot.net/
Thread model: posix
gcc version 4.6.3 (Buildroot 2012.11-git-00344-g2410e3d-dirty) 

       +--------------------------------+----------+
       |              68                |          |
       +--------------------------------|          |
       |              len               |   帧头   |
       +--------------------------------|          |
       |              len               |          |
       +--------------------------------|          |
       |              68                |          |
       +--------------------------------+ ---------+
       |            控制位              |          |
       +--------------------------------+          |
       |             地址               |          |
       +--------------------------------+----------+
       |             类型标示           |  数据    |
       +--------------------------------+  单元    |
       |       可变结构限定词            |  标示    |
       +--------------------------------+    符    |
       |            传送原因            |          |
       +--------------------------------+          |
       |               addr             |          |
       +--------------------------------+          |
       |               addr             |          |
       +--------------------------------+          |
       |               RAD              |          |
       +--------------------------------+----------+
       |              校验              |   帧尾
       +--------------------------------+
       |          结束符0x16            |
       +--------------------------------+
```

