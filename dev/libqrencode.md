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
