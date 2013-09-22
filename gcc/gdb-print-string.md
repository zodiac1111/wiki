# gdb打印完整字符串

* http://stackoverflow.com/questions/233328/how-do-i-print-the-full-value-of-a-long-string-in-gdb
```
set print elements 0
```
[From the GDB manual](http://ftp.gnu.org/old-gnu/Manuals/gdb-5.1.1/html_node/gdb_57.html#IDX353):
```
set print elements number-of-elements
```
Set a limit on how many elements of an array GDB will print. If GDB is printing a large array, it stops printing after it has printed the number of elements set by the set print elements command. This limit also applies to the display of strings. When GDB starts, this limit is set to 200. Setting number-of-elements to zero means that the printing is unlimited.