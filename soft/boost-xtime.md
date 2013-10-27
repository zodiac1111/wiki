# boost bug

来源:https://bbs.archlinux.org/viewtopic.php?pid=1126374

# 现象

表现如下:
```
In file included from /usr/include/boost/thread/pthread/mutex.hpp:14:0,
                 from /usr/include/boost/thread/mutex.hpp:16,
                 from /usr/include/boost/thread/pthread/thread_data.hpp:12,
                 from /usr/include/boost/thread/thread.hpp:17,
                 from /usr/include/boost/thread.hpp:13,
                 from /home/XXXX/pulseview/cotire/pulseview_CXX_prefix.hxx:14:
/usr/include/boost/thread/xtime.hpp:23:5: error: expected identifier before numeric constant
/usr/include/boost/thread/xtime.hpp:23:5: error: expected ‘}’ before numeric constant
/usr/include/boost/thread/xtime.hpp:23:5: error: expected unqualified-id before numeric constant
/usr/include/boost/thread/xtime.hpp:46:14: error: expected type-specifier before ‘system_time’
```
发生在boost1.50以前.(debian 7还是1.49的)

具体文件内容:
```
enum xtime_clock_types
{
    TIME_UTC=1  //<-23 line
//    TIME_TAI,
//    TIME_MONOTONIC,
//    TIME_PROCESS,
//    TIME_THREAD,
//    TIME_LOCAL,
//    TIME_SYNC,
//    TIME_RESOLUTION
};

```
#解决

方法1: 升级boost :)

方法2: 修改`TIME_UTC` 为 `TIME_UTC_`

> Just rename all the occurences of `TIME_UTC` to `TIME_UTC_` in the `/usr/include/boost/thread/xtime.hpp`. This is safe, since both boost definition of `TIME_UTC` and `glibc` definition of `TIME_UTC` are the same (ie. TIME_UTC = 1).