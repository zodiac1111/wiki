# rpc

什么是rpc [wiki](http://zh.wikipedia.org/wiki/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)

远程过程调用（Remote Procedure Call，RPC）是一个计算机通信协议。该协议允许运行于一台计算机的程序调用另一台计算机的子程序，而程序员无需额外地为这个交互作用编程。如果涉及的软件采用面向对象编程，那么远程过程调用亦可称作远程调用或远程方法调用，例：Java RMI。

# json-rpc

[JSON-RPC轻量级远程调用协议介绍及使用](http://gubaojian.blog.163.com/blog/static/1661799082012101439591/)

json-rpc是基于json的跨语言远程调用协议，

* 比xml-rpc、webservice等基于文本的协议**传输数据量小**；
* 相对hessian、java-rpc等二进制协议**便于调试、实现、扩展**，是非常优秀的一种远程调用协议。

目前主流语言都已有json-rpc的实现框架，java语言中较好的json-rpc实现框架有jsonrpc4j、jpoxy、json-rpc。三者之中jsonrpc4j既可独立使用，又可与spring无缝集合，比较适合于基于spring的项目开发。