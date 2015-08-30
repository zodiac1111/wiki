 日志
======

# 控制台输出彩色日志

[stackoverflow](http://stackoverflow.com/questions/27464492/sending-ansi-colored-codes-text-to-3-outputs-screen-file-and-file-filtering-an)

# 自定义终端提示符

http://venus585625.iteye.com/blog/1174567

# 数组长度

头文件include/linux/kernel.h包含了一些宏
如果你需要计算一个数组的长度，使用这个宏

    #define ARRAY_SIZE(x) (sizeof(x) / sizeof((x)[0]))

如果你要计算某结构体成员的大小，使用

    #define FIELD_SIZEOF(t, f) (sizeof(((t*)0)->f))

# 时间戳表示法

    yyyy-MM-dd HH:mm:ss,dddd 

2013-04-15 00:20:41,星期一

2013-04-15 00:21:04,星期一 +08:002013-04-15 00:21:21+08:00,星期一

# 字符串宏

    #define xstr(s) #s

# 调试宏

[代码](https://github.com/zodiac1111/c_call_lua)

```
#if 1 /// clog debug
/// 前缀CL  c log
#  include <time.h>
#  include <errno.h>
#  define CL_ATTR_RED "\033[31m"
#  define CL_ATTR_GREEN "\033[32m"
#  define CL_ATTR_YELLOW "\033[33m"
#  define CL_ATTR_BLUE "\033[34m"
#  define CL_ATTR_PURPLE "\033[35m"
#  define CL_ATTR_REV "\033[7m"
#  define CL_ATTR_UNDERLINE "\033[4m"
#  define CL_ATTR_END "\033[0m"
/// *** 自定义输出的换行, 空/ NL 等 *****
#  define CL_LINE_NULL ""
#  define CL_LINE_END_WIN "\r\n"
#  define CL_LINE_END_LINUX "\n"
#  define CL_LINE_END_MAC "\r"
#  define CL_NEWLINE CL_LINE_END_LINUX
/// 简单的分级别调试,只要头文件.根据需要可以去挑error库
#  define CLOG_DEBUG(fmt, ...) \
		fprintf(stdout,\
		"I "CL_ATTR_PURPLE"%s (%s:%d):%s[%d] "CL_ATTR_END fmt CL_NEWLINE\
		,__FUNCTION__,__FILE__, __LINE__ \
		,strerror(errno) ,errno\
		,##__VA_ARGS__ )
#  define CLOG_INFO(fmt, ...) \
		fprintf(stdout,\
		"I "CL_ATTR_GREEN"%s (%s:%d):%s[%d] "CL_ATTR_END fmt CL_NEWLINE\
		,__FUNCTION__,__FILE__, __LINE__ \
		,strerror(errno) ,errno\
		,##__VA_ARGS__ )
#  define CLOG_WARN(fmt, ...) \
		fprintf(stdout,\
		"E "CL_ATTR_YELLOW"%s (%s:%d):%s[%d] "CL_ATTR_END fmt CL_NEWLINE\
		,__FUNCTION__,__FILE__, __LINE__ \
		,strerror(errno) ,errno\
		,##__VA_ARGS__ )
#  define CLOG_ERR(fmt, ...) \
		fprintf(stdout,\
		"E " CL_ATTR_RED "%s (%s:%d):%s[%d] "CL_ATTR_END fmt CL_NEWLINE\
		,__FUNCTION__,__FILE__, __LINE__ \
		,strerror(errno) ,errno\
		,##__VA_ARGS__ )
#else
#  define CLOG_DEBUG(fmt, ...)
#  define CLOG_INFO(fmt, ...)
#  define CLOG_WARN(fmt, ...)
#  define CLOG_ERR(fmt, ...)
#endif /** clog debug */
```
