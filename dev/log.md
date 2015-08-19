# 日志

控制台输出彩色日志

http://stackoverflow.com/questions/27464492/sending-ansi-colored-codes-text-to-3-outputs-screen-file-and-file-filtering-an

# 数组长度

头文件include/linux/kernel.h包含了一些宏
如果你需要计算一个数组的长度，使用这个宏

    #define ARRAY_SIZE(x) (sizeof(x) / sizeof((x)[0]))

如果你要计算某结构体成员的大小，使用

    #define FIELD_SIZEOF(t, f) (sizeof(((t*)0)->f))
