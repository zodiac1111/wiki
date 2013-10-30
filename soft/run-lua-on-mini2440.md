# 移植lua到mini2440

原本是玩游戏第一次接触到lua外挂,之后在wireshark的插件中也有所涉及.今天没事想看看lua怎么实现的.发现官网源码包才几百k,下下来把弄了一下.一眼看到旁边地上积灰的mini2440,于是弄上去玩玩.各种折腾花了我一下午的时间.

# 需要的东西

* 交叉编译器 就友善之臂给的呗,之前自己的用uclibc跟友善之臂的很难弄好.待等级高点再搞
* 开发板 mini2440 所有东西都是原来给的样子,启动时甚至触摸屏矫正程序都还在.
* lua源码5.2.2 官网下: http://www.lua.org/download.html
* readline6.2 和 ncurses5.9 都是GNU的,网站都有. http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html 和  http://www.gnu.org/software/ncurses/

#编译

## 编译 ncurses

可以网上搜索一下,有很多交叉编译的教程,下面是我的

下载解压后到源码跟目录,配置一下

```
./configure --host=arm-linux --prefix=$(pwd)/_install -with-shared
```

* `--host=arm-linux` 指定平台为 arm-linux,默认的makefile里的 CC CXX AR 变量等命令都会增加这个前缀,
* `--prefix=$(pwd)/_install` 默认安装在当前子目录下,没必要安装到host的系统目录嘛
* `-with-shared` 生成.so的共享库,不然仅生成.a的库.

`make && make install`

以下是我的小小不同,因为系统中有多个交叉编译器,没改变PATH变量,就手动指定了交叉编译器路径,各种编译器,ar,ranlib之类的

```
./configure --host=arm-linux --prefix=$(pwd)/_install CC=/home/zodiac1111/Documents/mini2440-2011110/linux/opt/FriendlyARM/toolschain/4.4.3/bin/arm-linux-gcc AR=/home/zodiac1111/Documents/mini2440-2011110/linux/opt/FriendlyARM/toolschain/4.4.3/bin/arm-linux-ar LD=/home/zodiac1111/Documents/mini2440-2011110/linux/opt/FriendlyARM/toolschain/4.4.3/bin/arm-linux-ld RANLIB=/home/zodiac1111/Documents/mini2440-2011110/linux/opt/FriendlyARM/toolschain/4.4.3/bin/arm-linux-ranlib CXX=/home/zodiac1111/Documents/mini2440-2011110/linux/opt/FriendlyARM/toolschain/4.4.3/bin/arm-linux-g++ -with-shared
```


## 编译 readline

下载解压转到目录
```
./configure --host=arm-linux CC=/home/zodiac1111/Documents/mini2440-2011110/linux/opt/FriendlyARM/toolschain/4.4.3/bin/arm-linux-gcc --prefix=$(pwd)/_install
```
`make && make install`

## 编译 lua

编辑makefile文件,或者直接指定编译器.因为还有该一些include和lib路径,所有我就修改了makefile文件.


`MYCFLAGS`增加刚才instll的2个头文件的目录,让lua编译的时候可以找到他们而不是找到其他奇怪的地方(比如host系统的头文件)去:

```
MYCFLAGS= -I/home/zodiac1111/Downloads/readline-6.2/_install/include \
	-I/home/zodiac1111/Downloads/ncurses-5.9/_install/include \
	-DLUA_USE_POSIX -DLUA_USE_DLOPEN
```

`MYLDFLAGS`也增加,让lua好找到我们编译出来的so库文件:

```
MYLDFLAGS=  -L/home/zodiac1111/Downloads/readline-6.2/_install/lib \
	-L/home/zodiac1111/Downloads/ncurses-5.9/_install/lib
```

以下这个地方添加`-lncurses`,貌似是他们漏了这个库的依赖,网上也有人指出了.

```
linux:
	$(MAKE) $(ALL) SYSCFLAGS="-DLUA_USE_LINUX" SYSLIBS="-Wl,-E -ldl -lreadline -lncurses"
```

开始编译lua吧.

```
make linux
```

# 目标版

lua,readline和ncurses的库传到开发板上,我就用原始的ftp.

库放到库的位置,建立好符号链接,看看其他库怎么做的,照样画葫芦呗.注意可执行权限有没有.

运行lua就出来熟悉的画面啦

```
[root@FriendlyARM plg]# lua 
Lua 5.2.2  Copyright (C) 1994-2013 Lua.org, PUC-Rio     
> print("hello,world!\n")
hello,world!

> 
```