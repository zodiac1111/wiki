读取函数符号表

http://stackoverflow.com/questions/4514745/how-do-i-view-the-list-of-functions-a-linux-shared-library-is-exporting

```
nm -D 1.so
objdump -T 1.so
```