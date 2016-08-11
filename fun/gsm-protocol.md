# gsm 协议中的流程

#名词解释

```
MS　: mobile station  移动终端
MSI：International Mobile Subscriber Identity，国际移动用户标识号，是TD系统分给用户的唯一标识号，它存储在SIM卡、HLR/VLR中，最多由15个数字组成；
MCC：Mobile Country Code，是移动用户的国家号，中国是460；
MNC：Mobile Network Code ，是移动用户的所属PLMN网号，中国移动为00、02，中国联通为01；
MSIN：Mobile Subscriber Identification Number，是移动用户标识；
NMSI：National Mobile Subscriber Identification，是在某一国家内MS唯一的识别码；
BTS：Base Transceiver Station，基站收发器；
BSC：Base Station Controller，基站控制器；
MSC：Mobile Switching Center，
移动交换中心。移动网络完成呼叫连接、过区切换控制、无线信道管理等功能的设备，同时也是移动网与公用电话交换网(PSTN)、综合业务数字网(ISDN)等固定网的接口设备；
HLR：Home location register。保存用户的基本信息，如你的SIM的卡号、手机号码、签约信息等，和动态信息，如当前的位置、是否已经关机等；
VLR：Visiting location register，保存的是用户的动态信息和状态信息，以及从HLR下载的用户的签约信息；
CCCH：Common Control CHannel，公共控制信道。是一种“一点对多点”的双向控制信道，其用途是在呼叫接续阶段，传输链路连接所需要的控制信令与信息。
```

SDCCH : MS向系统请信令信道

RR(Radio Resource Management)：channel, cell控制等信息，可以忽略
MM(Mobility Management)：Location updating（如果需要接收方号码，需要关注这个动作）
CM(Connection Management)：Call Control（语音通话时的控制信息，可以知道何时开始捕获TCH）, SMS（这里的重点）

Address Field，Control Field，Length Field
N(S) Send sequence number 
N(R) 是 Receive sequence number
TP-Originating-Address 就是发送者的号码

# 参考资料

[资料1](http://bbs.pediy.com/showthread.php?t=182574)

[瑞典皇家理工学院教程](https://people.kth.se/~johanmon/attic/2g1723/lectures.html)

[GSM详细资料协议栈](http://blog.csdn.net/xzl04/article/details/4147459)

# 等待迁移
男人你懂？
http://ww3.sinaimg.cn/large/49646b5egw1f6ndxi2a50j20f00k077e.jpg
http://ww4.sinaimg.cn/large/49646b5egw1f6ndxmc8h1j20e10kfjtx.jpg
http://ww3.sinaimg.cn/large/49646b5egw1f6ndxfpbk0j20fo0fodiw.jpg
http://ww4.sinaimg.cn/large/49646b5egw1f6ndxowp2vj20fo0d0tas.jpg
http://ww3.sinaimg.cn/large/49646b5egw1f6ndxtljxcj20fo0fo41n.jpg
http://ww3.sinaimg.cn/large/49646b5egw1f6ndxw8jeej20fo0a00ut.jpg
http://ww2.sinaimg.cn/large/49646b5egw1f6ndy1depfj20bd0qowh4.jpg

