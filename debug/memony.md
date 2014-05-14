# 开发用内存检测工具

# 内存检测

内存泄露

* DrMemory:http://www.ibm.com/developerworks/cn/linux/1309_liuming_drmemory/index.html?ca=drs-
* Valgrind 一系列内存检测工具(让程序飞系列) 
 * 介绍 http://www.osfans.org/category/tools/
 * 转载 http://www.haogongju.net/art/1534535
 * 转载2 http://www.cnblogs.com/lexus/archive/2011/11/13/2247508.html
 * 转载 http://blog.csdn.net/zhangm168/article/details/6286133
 * 转载 http://blog.csdn.net/powersaven/article/details/9466671
* Purify IBM $3000+ ...

查看占用内存

> http://bbs.csdn.net/topics/390184624

/proc/pid/maps pid为进程号，显示当前进程所占用的虚拟地址。

/proc/pid/statm 进程所占用的内存

# 调用检测
callgrind 
```
valgrind --tool=callgrind  ./1.cgi
callgrind_annotate --auto=yes callgrind.out.7850 >log
vi log
```
图形化的 KCachegrind

# 线程竞态检测

http://blog.chinaunix.net/uid-8625039-id-3583808.html

```
valgrind --tool=helgrind --log-file=helgrind.log ./helgrind_test
```

# 堆内存监视

Massif
```
valgrind --tool=massif ./test
```
查看:
* tui:`ms_print massif.out.<pid>`
* gui:http://stackoverflow.com/questions/1623771/valgrind-massif-tool-output-graphical-interface