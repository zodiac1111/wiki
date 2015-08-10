# CoAP

同类协议[mqtt](/dev/mqtt)

基于udp,更小,起草阶段.类比http协议的理念.来历未知

# 实现

* 有名的小巧c实现:[libcoap](http://sourceforge.net/projects/libcoap/)
* [arduino](https://github.com/dgiannakop/Arduino-CoAP)
* [arduino](https://github.com/1248/microcoap)

# libcoap

## 编译

### host编译

    ./configure
    make

### 交叉编译

    ./configure
    make CC=arm-linux-gcc AR=arm-linux-ar LD=arm-linux-ld

examples下coap-client就是客户端例子

## 测试

    ./examples/coap-client coap://coap.me/.well-known/core

默认支持的ipv6似乎关不掉

wireshark 过滤

    udp.port==5683

# 参考

## 英文

* [coap](http://coap.technology/)
* [协议文档rfc7252](http://tools.ietf.org/html/rfc7252)
* [各种语言的实现](https://en.wikipedia.org/wiki/Constrained_Application_Protocol)
* [arm公司的介绍幻灯片](http://www.slideshare.net/zdshelby/coap-tutorial)

## 中文

* [CoAP学习笔记——IETF文档和draft文档](http://blog.csdn.net/xukai871105/article/details/45097841)
* [多协议物联网演示](https://github.com/iot-works/diaonan)
* [基于CoAP的无线传感器网络与互联网的互联研究](http://www.itcast.cn/news/20140606/14323875164.shtml)
* [CoAP学习笔记——Libcoap安装和使用](http://blog.csdn.net/xukai871105/article/details/44980041)
* [构建基于CoAP协议的物联网系统(图灵社区)](http://www.ituring.com.cn/tupubarticle/3795)
* [介绍](http://blog.csdn.net/tulun/article/details/8869241)
* [译:CoAP协议学习——CoAP基础](http://blog.csdn.net/xukai871105/article/details/17734163)
* [CoAP与物联网系统](http://www.phodal.com/blog/use-constrained-application-protocol-in-internet-of-things/)