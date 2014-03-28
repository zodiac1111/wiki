# 交叉编译libmodbus

配置

    ./configure --host=arm-linux \
    --prefix='/home/zodiac1111/Downloads/libmodbus-3.1.1/_install' 

编译

    make && make install

# 使用

_install目录下lib是库文件,include头文件,test目录有使用api的例子