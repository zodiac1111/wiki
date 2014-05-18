# 修改正在运行的程序

参考 http://www.newsmth.net/nForum/#!article/LinuxDev/63264?p=2

```bash
替换正在运行的程序/正在使用的动态库，使用 install 命令，不要用 cp 
```

```bash
因为楼主想更新正在运行的可执行文件/动态库…… 
  
你可以试试直接 open+write 是啥结果，要么是 text file busy（可执行文件），要么原来的进程崩溃（动态库） 
```