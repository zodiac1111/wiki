# 编译libwebsockets

# 下载

> http://libwebsockets.org/trac/libwebsockets

```
git clone git://git.libwebsockets.org/libwebsockets
```
```
目前版本
2013-12-21 11:18 Andy Green         o [master] [origin/HEAD] [origin/master] unify all pollfd lock management
```

# 编译

 使用cmake -> make 编译

## 默认
安装到本机默认位置 /usr/local/bin/
```
cd cmake
cmake ..
make && sudo make install
```

## 指定安装路径
安装在项目_install文件夹下
```
cd cmake
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=../_install 
make && make install
```

# 测试

`_install/share/libwebsockets-test-server`1目录是web页面目录.

运行`libwebsockets-test-server`.

浏览器打开`127.0.0.1:7681`打开测试页面.

# 独立

上一步编译出so文件.使用so文件.

`test-server`下是一些测试程序.

独立测试:
```
gcc  test-server.c -lwebsockets
```

#交叉编译(arm/mini2440)

按照官方的说法:
```
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=/usr \
	 -DCMAKE_TOOLCHAIN_FILE=../cross-arm-linux-gnueabihf.cmake \
	 -DWITHOUT_EXTENSIONS=1 -DWITH_SSL=0
```

改几个参数:

* CMAKE_TOOLCHAIN_FILE 这个我喜欢安装在 ../_install目录.方便复制
* CMAKE_TOOLCHAIN_FILE 这个就原来那样,但是修改cross-arm-linux-gnueabihf.cmake文件内容.指定自己的编译器.

编辑 cross-arm-linux-gnueabihf.cmake 文件,修改 CROSS_PATH 指向编译器根目录.

执行cmake .

说UseRPMTools问题,我不需要.注销掉
```
CMake Error at CMakeLists.txt:762 (INCLUDE):
  include could not find load file:

    UseRPMTools
```
CMakeLists.txt:762这里找到,注销这个判断.

make

make install