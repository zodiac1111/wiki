# 有趣的东西

## 虚拟Linux
* Javascript实现的最小化的Linux虚拟机 http://s-macke.github.io/jor1k/
* http://bellard.org/jslinux/ 不开源 

## 树莓派
* 打造无广告的无线接入点(AP) http://learn.adafruit.com/raspberry-pi-as-an-ad-blocking-access-point/overview

* 簡易線上LED計算工具,计算需要的电阻 http://led.linear1.org/1led.wiz
* avr项目peter 有价值的参考网站 http://homepage.hispeed.ch/peterfleury/
* minecraft控制现实世界的红石灯 http://www.hive76.org/real-life-replica-redstone-lamp-controlled-by-a-raspberry-pi

## 3d
* github上的3d图形差异演示 https://github.com/skalnik/peg-board-spindle/commit/7a1039fe5709ff49eec7800aa7259a5e5b536d05#diff-0

## 神

* 混乱c语言大赛, 8086个半字节的虚拟机.(最小的虚拟机) http://ioccc.org/2013/cable3/hint.html

## x!=x

> * http://blog.chinaunix.net/uid-23629988-id-3126229.html
> * http://blog.chinaunix.net/uid-23629988-id-3149291.html

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    float x;
    int a = 1/0.0;//inf
    int a = 0/0.0;//nan
    memcpy(&x, &a, sizeof(x));
    if (x == x) {
        printf("Equal\n");
    } else {
        printf("Not equal\n");
    }

    if (x >= 0.0) {
        printf("x(%f) >= 0\n", x);
    } else if (x < 0.0) {
        printf("x(%f) < 0\n", x);
    } else {
        printf("Surprise x(%f)!!!\n", x);
    }


    return 0;
}
```
非法的另一种来源
```c
#include <stdlib.h>
#include <stdio.h>

int main(void)
{
    float x;

    while (1) {
        scanf("%f", &x);
        printf("x is %f\n", x);
    }

    return 0;
}
```
```bash
[fgao@fgao-vm-fc13 test]$ ./a.out
inf
x is inf
nan
x is nan
```
一种判断方式
http://www.opensource.apple.com/source/Libc/Libc-167/gen.subproj/ppc.subproj/isinf.c