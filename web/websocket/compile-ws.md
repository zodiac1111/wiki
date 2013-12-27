# 编译libwebsockets

# 下载

```
git clone git://git.libwebsockets.org/libwebsockets
```
```
目前版本
2013-12-21 11:18 Andy Green         o [master] [origin/HEAD] [origin/master] unify all pollfd lock management
```

# cmake

## 默认
安装到本机默认未知 /usr/local/bin/
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
