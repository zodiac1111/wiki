# 移除共享内存

## 在bosybox中使用,不支持xarg的-i选项

    ipcs -m|grep 777|awk '{print $2}'|while read foo; do ipcrm -m $foo; done

* `ipcs -m` 查找共享neic
* `grep 777` 过滤出我自己创建的(丑陋的方式)
* `awk '{print $2}'` 打印第二列, 共享内存的标志列
* `while read foo; do xxxxx; done` 一个循环读入,
* `ipcrm -m $foo` 移除一个共享内存

## 一般使用

    ipcs -m |grep 777|awk '{print $2}'|xargs -i ipcrm -m {}

* 其他一样
* `xargs -i ipcrm -m {}` 同上面的while循环. {}表示参数(foo)

## makefile中

    ipcs -m |grep 777|awk '{print $$2}'|xargs -i ipcrm -m {}

* 其他一样
* `awk '{print $$2}'` 注意两个$,一个$会被转意