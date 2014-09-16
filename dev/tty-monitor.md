# 各种方式的串口监视

# strace

```shell
strace -s9999 -o myapp.strace -eread,write,ioctl ./myapp
```

返回类似
```text
```


优点

* 不烦扰原来程序的运行,透明监视
* 保存文件

缺点

* c整合可能不太方便
* 不能不影响运行的情况下启动/关闭监视
* 输出字符串
