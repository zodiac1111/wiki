# Golden Dict

Linux下的词典,

* 官网 http://goldendict.org/
* 源代码 https://github.com/goldendict

编译安装 

Make sure you have those dependency packages installed: `libvorbis-dev`, `zlib1g-dev`, `libhunspell-dev`, `x11proto-record-dev`, `qt4-qmake`, `libqt4-dev`, `g++`, `libxtst-dev`, `libphonon-dev`, `liblzo2-dev`, `libbz2-dev`, `libao-dev`, `libavutil-dev`, `libavformat-dev`. They can be named slightly different in different distributions.

```text
$ git clone git://github.com/goldendict/goldendict.git
$ cd goldendict && qmake && make

Make sure you use qmake from the qt4 install. You can then optionally do make install under root to install the program system-wide, or just run it directly from the build directory
不同的发行版名称可能不一样,以上是在Debian系的名字
```

如果提示缺少一些包,安装它
```bash
zodiac1111@debian:goldendict$ ./configure 

To build the program, run qmake, then make.

The following dependency packages are required: libvorbis-dev, zlib1g-dev, libhunspell-dev, x11proto-record-dev, qt4-qmake, libqt4-dev, g++, libxtst-dev, libphonon-dev. They can be named slightly different in different distributions.
zodiac1111@debian:goldendict$ qmake
Project MESSAGE: Install Prefix is: /usr/local
Project ERROR: Package ao not found
zodiac1111@debian:goldendict$ aot-get install libao-dev

```
目前可能还需要的包:`libao-dev`,`libavutil-dev`,`libeb16-dev`,`liblzo2-dev`,`libtiff5-dev`