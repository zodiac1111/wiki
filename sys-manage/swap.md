# swap 使用情况

为什么物理内存没用完就用Swap分区了? http://bbs.chinaunix.net/thread-4118465-1-1.html


http://bbs.chinaunix.net/thread-3617338-1-1.html

Linux 内核 2.6 版本后引入的
`/proc/sys/vm/swappiness`

The Linux 2.6 kernel added a new kernel parameter called swappiness to let administrators tweak the way Linux swaps.
https://www.linux.com/news/softw ... ut-linux-swap-space

默认是60
如果设置为 0 的话，则等同于禁用 swap

A value of 0 will avoid ever swapping out just for caching space. Using 100 will always favor making the disk cache bigger. Most distributions set this value to be 60, tuned toward moderately aggressive swapping to increase disk cache. 
http://www.westnet.com/~gsmith/content/linux-pdflush.htm

这样的话，不管创建多大或多小的swap分区，都形同虚设了是不？？


>The default value I’ve seen on both enterprise level Red Hat and SLES servers is 60.
>
>To find out what the default value is on a particular server, run:
>
>`sysctl vm.swappiness`
>The value is also located in `/proc/sys/vm/swappiness`.
