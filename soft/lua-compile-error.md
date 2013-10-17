#编译lua失败

来源: 

* http://guiquanz.github.io/2013/04/29/lua-5.2.2-compiling-error/
* http://lua-users.org/lists/lua-l/2013-02/msg00456.html 
* http://lua-users.org/lists/lua-l/2013-02/msg00488.html 

Lua-5.2.2在redhat Linux平台编译失败解决 Apr 29, 2013

lua-5.2.2发布已有一段时间了，最近在redhat Linux平台编译时报错。这里给出解决方案，或许对某人会有帮助。

编译报错，如下：

```
lua@home> make linux
 ...
gcc -O2 -Wall -DLUA_COMPAT_ALL -DLUA_USE_LINUX    -c -o lstrlib.o lstrlib.c
gcc -O2 -Wall -DLUA_COMPAT_ALL -DLUA_USE_LINUX    -c -o ltablib.o ltablib.c
gcc -O2 -Wall -DLUA_COMPAT_ALL -DLUA_USE_LINUX    -c -o loadlib.o loadlib.c
gcc -O2 -Wall -DLUA_COMPAT_ALL -DLUA_USE_LINUX    -c -o linit.o linit.c
ar rcu liblua.a lapi.o lcode.o lctype.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o         lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundump.o lvm.o lzio.o lauxlib.o lbaselib.o lbitlib.o     lcorolib.o ldblib.o liolib.o lmathlib.o loslib.o lstrlib.o ltablib.o loadlib.o linit.o 
ranlib liblua.a
gcc -O2 -Wall -DLUA_COMPAT_ALL -DLUA_USE_LINUX    -c -o lua.o lua.c
gcc -o lua   lua.o liblua.a -lm -Wl,-E -ldl -lreadline 
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `PC'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `tgetflag'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `tgetent'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `UP'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `tputs'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `tgoto'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `tgetnum'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `BC'
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../lib64/libreadline.so: undefined reference to `tgetstr'
collect2: ld returned 1 exit status
make[1]: *** [lua] Error 1
make[1]: Leaving directory `/home/lua/lua-5.2.2/src'
make: *** [linux] Error 2
```

由于lua编译依赖readline库，而其依赖ncurses库，但没有指定，所以出现“未定义的符合引用”错误。需要修改${LUA_DIR}/src/Makefile中linux编译target，在SYSLIBS变量中追加`-lncurses`选项即可。修改后，如下：
```
linux:
        $(MAKE) $(ALL) SYSCFLAGS="-DLUA_USE_LINUX" SYSLIBS="-Wl,-E -ldl -lreadline -lncurses"
 ```