# 有趣的东西

* IP over ICMP,DNS,etc. http://thomer.com/icmptx/
* x进制

* [各种稀奇古怪的进制](jinzhi)

语义饱和

* [语义饱和](http://zh.wikipedia.org/zh/%E8%AF%AD%E4%B9%89%E9%A5%B1%E5%92%8C)

破解

* [面试时被要求破解(crack)一个程序](fun/crack),([原文](http://erenyagdiran.github.io/I-was-just-asked-to-crack-a-program-Part-1/))

ATM

* [ATM读卡,密码截取装置(分离器)](http://krebsonsecurity.com/all-about-skimmers/)

重力模拟

* [浏览器模拟重力](http://www.nowykurier.com/toys/gravity/gravity.html)

风力仿生,步行,多足,机械

* [风力仿生兽](http://www.strandbeest.com/beests_leg.php)
* [风力仿生兽2](http://knewone.com/things/da-ren-de-ke-xue-xi-lie-di-30-qi-feng-li-fang-sheng-shou/reviews/54029e0431302d6cc1fc0700)

标签打印机

* [迷你标签打印机](http://www.chiphell.com/thread-1110371-1-1.html)
* [另一款brother](http://www.chiphell.com/thread-856599-1-1.html)
* [机械模型/发动机/德国](http://www.en.boehm-stirling.com/)
 
## 极小的等宽英文字符集
```text
 一种极小的等宽英文字符集 3x5 像素 szdiy
Atommann <atommann@gmail.com>
13:25 (3小时前)

发送至 Shenzhen 
Hi all,

字体在这里
http://robey.lag.net/2010/01/23/tiny-monospace-font.html

这么小的字体可以用来给 IC 标引脚
http://notanumber.net/archives/83/label-your-chips

也可以用在 LED 点阵上。
```

## 可视化算法

* [可视化算法1](http://www.comp.nus.edu.sg/~stevenha/visualization/index.html) 用作教育不错
* [算法可视化](http://bindog.github.io/%E7%90%86%E8%AE%BA/2014/08/09/visualizing-algorithms/)
* [有趣的话](talk)
* 你想过如何通过程序—创作、处理、分析音乐吗？([github.io](http://music-suite.github.io/docs/ref/))
* 一种编码[ECMA](http://www.polyomino.org.uk/computer/ECMA-10/),[wiki]()
* 开源无线电,飞机基站等[中文博客](http://blog.sina.com.cn/s/blog_67cdafe201014odm.html).[本地备份](gnuradio)
* 欧姆龙环,纸币防伪 圆圈星座防伪技术 [链接](http://zh.wikipedia.org/zh/%E5%9C%86%E5%9C%88%E6%98%9F%E5%BA%A7%E9%98%B2%E4%BC%AA%E6%8A%80%E6%9C%AF)

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

## 计算机,虚拟化

* 混乱c语言大赛, 8086个半字节的虚拟机.(最小的虚拟机) http://ioccc.org/2013/cable3/hint.html

## 信息
* linux博客[[podcasts]]
* [[coreboot]]  快速开机 1.5秒
* 查询自己的浏览器 http://www.whatsmybrowser.org/
* 通过dns协议传输文件 http://www.aldeid.com/wiki/File-transfer-via-DNS
* Real-time server monitoring in your browser 在浏览器中实时监控你的服务器 http://scoutapp.github.io/scout_realtime/
* ia32rtools 反汇编,把x86汇编代码反编译成c语言.
* [Linuxbrew: Homebrew for Linux](https://linuxtoy.org/archives/linuxbrew.html)在 OS X 平台上非常流行的包管理器 Homebrew 最近正被移植 到 Linux 上而成为 Linuxbrew
* [用一句printf实现一个简单的web server](http://tinyhack.com/2014/03/12/implementing-a-web-server-in-a-single-printf-call/)
* [printf格式化字符串](http://crypto.stanford.edu/cs155/papers/formatstring-1.2.pdf)

* 天使与魔鬼字体双向体 http://www.flipscript.com/ambigram-font.aspx

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

# 数学

* 弧度演示 [[/res/img/How_radians_work.gif]]
* 一些其他数学方面的图片 https://en.wikipedia.org/wiki/User:LucasVB/Gallery [个人网站](http://1ucasvb.tumblr.com/)
* [图形化的数学,齿轮艺术](http://bugman123.com/Gears/index.html)

* 傅里叶变换演示 [[/res/img/Fourier_transform.gif]] http://1ucasvb.tumblr.com/post/43816237610/the-fourier-transform-takes-an-input-function-f