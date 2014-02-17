# lsof socket文件查看

> 参考:http://blog.csdn.net/kozazyh/article/details/5495532

# lsof命令是什么？

可以列出被进程所打开的文件的信息。被打开的文件可以是

1. 普通的文件
2. 目录  
3. 网络文件系统的文件，
4. 字符设备文件  
5. (函数)共享库  
6. 管道，命名管道 
7. 符号链接
8. 底层的socket字流，网络socket，unix域名socket
9. 在linux里面，大部分的东西都是被当做文件的…..还有其他很多