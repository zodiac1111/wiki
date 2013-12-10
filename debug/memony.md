# 开发用内存检测工具

# 内存检测

* DrMemory:http://www.ibm.com/developerworks/cn/linux/1309_liuming_drmemory/index.html?ca=drs-
* Valgrind 一系列内存检测工具

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
* tui:`ms_print massif.out.<pid>`
* gui:http://stackoverflow.com/questions/1623771/valgrind-massif-tool-output-graphical-interface