#libqrencode QRcode lib

官方网址 https://github.com/fukuchi/libqrencode

host compile:

```bash
./configure
make
make install
```

cross compile (arm)
```bash
./configure CC=arm-linux-gcc --host=arm --prefix='/home/zodiac1111/Downloads/qrencode-3.4.3/install'
make
make install
```

生成的文件:

```bash
zodiac1111@debian:qrencode-3.4.3$ tree install/
install/
├── bin
│   └── qrencode
├── include
│   └── qrencode.h
├── lib
│   ├── libqrencode.a
│   ├── libqrencode.la
│   └── pkgconfig
│       └── libqrencode.pc
└── share
    └── man
        └── man1
            └── qrencode.1

7 directories, 6 files
```

文件的格式:
```bash
zodiac1111@debian:qrencode-3.4.3$ file install/bin/qrencode 
install/bin/qrencode: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), not stripped

```

cross run:
```bash
[root@AT91SAM9-RT9x5 /data]# ./qrencode 
./qrencode: can't load library 'libpng14.so.14'  <- 没有libpng库,需要自己编译
```

# libpng cross compile

官方网址: http://www.libpng.org/pub/png/libpng.html

我选择1.2版本的编译,貌似是比较老的版本.debian目前还是这个版本. 1.6的没有编辑脚本,不会用 = =

参考 http://www.ridgesolutions.ie/index.php/2014/02/05/cross-compiling-libpng-for-arm-linux-with-neon-and-zlib/

```bash
./configure --host=arm-linux CC=arm-linux-gcc     AR=arm-linux-ar STRIP=arm-linux-strip RANLIB=arm-linux-ranlib --prefix='/home/zodiac1111/Downloads/libpng-1.2.51/install' 
```
文件:
```bash
zodiac1111@debian:libpng-1.2.51$ tree install
install
├── bin
│   ├── libpng12-config
│   └── libpng-config -> libpng12-config
├── include
│   ├── libpng12
│   │   ├── pngconf.h
│   │   └── png.h
│   ├── pngconf.h -> libpng12/pngconf.h
│   └── png.h -> libpng12/png.h
├── lib
│   ├── libpng12.a
│   ├── libpng12.la
│   ├── libpng12.so -> libpng12.so.0.51.0
│   ├── libpng12.so.0 -> libpng12.so.0.51.0
│   ├── libpng12.so.0.51.0
│   ├── libpng.a -> libpng12.a
│   ├── libpng.la -> libpng12.la
│   ├── libpng.so -> libpng12.so
│   ├── libpng.so.3 -> libpng.so.3.51.0
│   ├── libpng.so.3.51.0
│   └── pkgconfig
│       ├── libpng12.pc
│       └── libpng.pc -> libpng12.pc
└── share
    └── man
        ├── man3
        │   ├── libpng.3
        │   └── libpngpf.3
        └── man5
            └── png.5

9 directories, 21 files
```

复制`libpng12.so.0.51.0`到target,运行
```bash
[root@AT91SAM9-RT9x5 /data]# ./qrencode -o 1.png "hello"
libpng warning: Application was compiled with png.h from libpng-1.4.7
libpng warning: Application  is  running with png.c from libpng-1.2.51
libpng error: Incompatible libpng version in application and library
Failed to initialize PNG writer.
[root@AT91SAM9-RT9x5 /data]# 
```
咕~~(╯﹏╰)b,版本不对

重新编译`libqrencode`库

# 第二次编译了

configue后 找到makefile,把png14改成png12

编译好的png12 lib和include目录复制到交叉编译器的`/opt/rt9x5/arm-linux-uclibcgnueabi/usr/arm-unknown-linux-uclibcgnueabi/sysroot`目录下,注意文件结构一致.


然后target的png14改成png12库;
```bash
ln -s /lib/libpng14.so.14 /lib/libpng12.so.0
```