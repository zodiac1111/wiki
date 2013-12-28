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