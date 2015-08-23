# lua相关

# 跨平台轻量级调试器 

* [ZeroBrane Studio](https://studio.zerobrane.com/download.html)
* [调试lua代码](http://www.cnblogs.com/baiyanhuang/archive/2013/01/01/2841398.html)

# socket.http

```lua
local http = require("socket.http")
```

#字符串连接
```lua
val="a" .. "b"
```

# 解析html

https://github.com/wscherphof/lua-htmlparser

安装需要lua5.2

    luarocks install htmlparser

如果不知默认的版本,用下面的命令指定编译好的本地版本

    /usr/local/bin/luarocks  install htmlparser
    
可能需要

    apt-get install liblua5.2

# 软件仓库 luarocks

最新的需要lua5.2

目前新版本没有进debian仓库,需要自己编译

http://www.luarocks.org/

## 编译安装

    ./configure

需要lua头文件 lua.h

    apt-get isntall 

压缩需要
 
    apt-get isntall lua-zlib

# luasocket

套接字,3.0版本包含http等

    apt-get install lua-socket