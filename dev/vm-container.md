在线调试.

jLink之后,直接cpu就有硬件断点,但一般就4-8个,及其珍贵

JTAG是联合测试工作组（Joint Test Action Group）

仿真器,执行不同架构的cpu的指令
仿真器（Emulator），又称仿真程序，在软件工程中指可以使计算机或者其他多媒体平台（掌上电脑，手机）能够运行其他平台上的程序，常被错误的称为模拟器。仿真器多用于电视游戏和街机，也有一些用于掌上电脑。仿真器一般需要ROM才能执行，ROM的最初来源是一些原平台的ROM芯片，通过一些手段将原程序拷贝下来（这个过程一般称之为“dump”）然后利用仿真器加载这些ROM来实现仿真过程

Em Vm Co

x86平台 仿真 psp  (Based on MIPS R4000 32-bit Core) 不同处理器架构 qemu

bvboard就是跨架构的仿真器,效率奇低.运行arm指令.

虚拟机: x

Virtl vame box 同样处理器架构,不同操作系统 linux/windows/macos(不包括ppc版本)

容器 Containers:
IaaS（Infrastructure-as-a-Service，基础设施即服务）

环境复杂.比如一个网站的测试,代码只是一小部分,比如后端java版本,操作系统版本.各种数据库及版本依赖,全都一股脑装到集装箱里头,
build - ship - run .一处编译到处运行.(java) 对比c语言的是:一处编写/到处编译. 汇编就是 一处编写/处处编写

Docker (同一操作系统,至少目前 docker win和mac有beta版本)
不同运行空间,就像一个进程一样运行.namespace

Unikernel

 MirageOS  比如dns服务器,域名解析,比如只有人请求一个域名的时候,这个操作系统才启动,并提供服务,paas即服务.速度在40ms左右.
既然使用效率低,那平时就让他关闭着好了.反正只有不到1秒钟就可以启动起来

evernote:

https://www.evernote.com/shard/s175/sh/00423c2c-9f2c-42e0-b990-25c7567a2fe8/2bf5cddadc6b93619886c92cb5bc11cd
