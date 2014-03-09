# coreboot

key:linux快速开机,启动,LinuxBIOS,chromebook号称1.5秒开机

http://www.coreboot.org/

邮件列表:

来自遥远的建议：

昨天在和一个高手聊天，随口问道，空闲的设备可以做什么？

答：

当然coreboot啦，我曾经有一个非常古老的主板，然后把cmos换掉了，换个了大的，并且变成了“双层”[1]，然后就开始了coreboot的学习和研究，最后，将一个完整的gentoo内核放到了那片flash里面去了，现在那块主板在我这里正在当文件服务器。


这个建议我觉得挺好就转到这里来了。coreboot是一个未来的发展方向，chromebook号称1.5秒开机，也是因为广泛的应用了coreboot。现在大部分主板已经不在用并行的rom芯片当cmos了，而是用改用串行的flash，这样可以把这个flash加到很大，比如128M 因为串行flash各种容量之间针脚定义都是通用的。8针和16针的也是可以转换的。所以，极大的增加了灵活性。有点儿像是给路由器换flash的感觉。128M的空间可以放一个内核+base系统了。(如果你的文件系统和外设硬件不是很奇怪的话还是甩掉那个initrd.img吧。)

* 一些硬件改造工具请参见这里 http://www.coreboot.org/Developer_Manual/Tools 
* [1]双层flash 请参见这里 http://www.coreboot.org/Developer_Manual/Tools/Dual_Flash
* 更换加装基座的方法看这里  http://www.coreboot.org/Soldering_a_socket_on_your_board      
* 附带视频地址 ： http://www.archive.org/download/CorebootHackingHowToSolderAPlccSocketOnYourBoard/coreboot_hacking_how_to_solder_a_plcc_socket_on_your_board_small.ogv

更多的视频看这里：http://www.coreboot.org/Screenshots#Videos
