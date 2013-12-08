# 开发用内存检测工具

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