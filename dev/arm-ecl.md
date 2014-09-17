# 在arm上运行lisp

[ecl:embedded common lisp](http://ecls.sourceforge.net/)

## uname

```text
zodiac1111@debian:ecl-13.5.1$ uname -a
Linux debian 3.14-2-amd64 #1 SMP Debian 3.14.15-2 (2014-08-09) x86_64 GNU/Linux

```
## gcc
```text
zodiac1111@debian:ecl-13.5.1$ gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc/x86_64-linux-gnu/4.9/lto-wrapper
Target: x86_64-linux-gnu
Configured with: ../src/configure -v --with-pkgversion='Debian 4.9.1-11' --with-bugurl=file:///usr/share/doc/gcc-4.9/README.Bugs --enable-languages=c,c++,java,go,d,fortran,objc,obj-c++ --prefix=/usr --program-suffix=-4.9 --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --with-gxx-include-dir=/usr/include/c++/4.9 --libdir=/usr/lib --enable-nls --with-sysroot=/ --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --enable-gnu-unique-object --disable-vtable-verify --enable-plugin --with-system-zlib --disable-browser-plugin --enable-java-awt=gtk --enable-gtk-cairo --with-java-home=/usr/lib/jvm/java-1.5.0-gcj-4.9-amd64/jre --enable-java-home --with-jvm-root-dir=/usr/lib/jvm/java-1.5.0-gcj-4.9-amd64 --with-jvm-jar-dir=/usr/lib/jvm-exports/java-1.5.0-gcj-4.9-amd64 --with-arch-directory=amd64 --with-ecj-jar=/usr/share/java/eclipse-ecj.jar --enable-objc-gc --enable-multiarch --with-arch-32=i586 --with-abi=m64 --with-multilib-list=m32,m64,mx32 --enable-multilib --with-tune=generic --enable-checking=release --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu
Thread model: posix
gcc version 4.9.1 (Debian 4.9.1-11) 
```

# arm-linux-gcc
```text
zodiac1111@debian:ecl-13.5.1$ arm-linux-gcc -v
使用内建 specs。
目标：arm-unknown-linux-uclibcgnueabi
配置为：/home/ldsh/rt9x5/linux/buildroot/buildroot/output/toolchain/gcc-4.3.5/configure --prefix=/opt/rt9x5/arm-linux-uclibcgnueabi/usr --build=i686-pc-linux-gnu --host=i686-pc-linux-gnu --target=arm-unknown-linux-uclibcgnueabi --enable-languages=c,c++ --with-sysroot=/opt/rt9x5/arm-linux-uclibcgnueabi/usr/arm-unknown-linux-uclibcgnueabi/sysroot --with-build-time-tools=/opt/rt9x5/arm-linux-uclibcgnueabi/usr/arm-unknown-linux-uclibcgnueabi/bin --disable-__cxa_atexit --enable-target-optspace --disable-libgomp --with-gnu-ld --disable-libssp --disable-multilib --disable-tls --enable-shared --with-gmp=/opt/rt9x5/arm-linux-uclibcgnueabi/usr --with-mpfr=/opt/rt9x5/arm-linux-uclibcgnueabi/usr --enable-threads --disable-decimal-float --with-float=soft --with-abi=aapcs-linux --with-arch=armv5te --with-tune=arm926ej-s --with-pkgversion='Buildroot 2011.05-dirty' --with-bugurl=http://bugs.buildroot.net/ : (reconfigured) /home/ldsh/rt9x5/linux/buildroot/buildroot/output/toolchain/gcc-4.3.5/configure --prefix=/opt/rt9x5/arm-linux-uclibcgnueabi/usr --build=i686-pc-linux-gnu --host=i686-pc-linux-gnu --target=arm-unknown-linux-uclibcgnueabi --enable-languages=c,c++ --with-sysroot=/opt/rt9x5/arm-linux-uclibcgnueabi/usr/arm-unknown-linux-uclibcgnueabi/sysroot --with-build-time-tools=/opt/rt9x5/arm-linux-uclibcgnueabi/usr/arm-unknown-linux-uclibcgnueabi/bin --disable-__cxa_atexit --enable-target-optspace --disable-libgomp --with-gnu-ld --disable-libssp --disable-multilib --disable-tls --enable-shared --with-gmp=/opt/rt9x5/arm-linux-uclibcgnueabi/usr --with-mpfr=/opt/rt9x5/arm-linux-uclibcgnueabi/usr --enable-threads --disable-decimal-float --with-float=soft --with-abi=aapcs-linux --with-arch=armv5te --with-tune=arm926ej-s --with-pkgversion='Buildroot 2011.05-dirty' --with-bugurl=http://bugs.buildroot.net/
线程模型：posix
gcc 版本 4.3.5 (Buildroot 2011.05-dirty) 
```