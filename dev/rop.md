# 返回导向编程 Return-oriented programming

缓冲区溢出攻击

> http://en.wikipedia.org/wiki/Return-oriented_programming 维基百科

相相似的还有 string-oriented programming

* 简介,实际例子,推荐 http://codearcana.com/posts/2013/05/28/introduction-to-return-oriented-programming-rop.html

# Return-into-libc 攻击 缓冲区溢出攻击

本文首先分析了 return-into-libc 的攻击原理，分别介绍了在不同平台进行传统 return-into-libc 攻击的实验过程和结果。然后，本文进一步引入并解释了返回导向编程的攻击方式，这种攻击可以弥补传统 return-into-libc 攻击的不足，使得攻击更灵活、更有效。最后，本文给出了针对这些攻击方法的防御手段。本文可以帮助读者了解 return-into-libc 攻击以及如何在系统中防止攻击的发生。


http://www.ibm.com/developerworks/cn/linux/1402_liumei_rilattack/index.html?ca=drs-