# dlopen动态加载so库

# 打开同一个so多次

参考: http://stackoverflow.com/questions/3433522/loading-two-instances-of-a-shared-library

默认多次打开都是用一个实例.里面函数地址都是相同的.%p打印可以证明.

window/linux 简单的重命名so库,分别调用是可以实现加载多个实例的.(脏)


一种方式 dlmopen glibc(uclibc不支持!)

