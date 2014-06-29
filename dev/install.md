# 修改正在运行的程序

参考 http://www.newsmth.net/nForum/#!article/LinuxDev/63264?p=2

```bash
替换正在运行的程序/正在使用的动态库，使用 install 命令，不要用 cp 
```

```bash
因为楼主想更新正在运行的可执行文件/动态库…… 
  
你可以试试直接 open+write 是啥结果，要么是 text file busy（可执行文件），要么原来的进程崩溃（动态库） 
```

# 使用cp覆盖的问题

对比cp的介绍

http://blog.sina.com.cn/s/blog_622a99700100pjv3.html

> install 的方式跟cp不同，先unlink再creat，当unlink的时候，已经map的虚拟空间vma中的inode结点没有变，只有inode结点的引用数为0是，kernel才把它干掉。
> 也就是新的so和旧的so用的不是同一个inode结点，所以不会相互影响。这时只有得启程序才会使用到新的so。所以采用这种方式的话就可以避先stop进程，更新so，再重启进程这样比较耗时的操作。

实验 http://www.linuxidc.com/Linux/2011-01/31622p2.htm

也是实际例子 http://www.piao2010.com/%E4%B8%BA%E4%BD%95cp%E8%A6%86%E7%9B%96%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%8A%A8%E6%80%81%E5%BA%93so%E4%BC%9A%E5%AF%BC%E8%87%B4coredump