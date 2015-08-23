# pjl/pcl

[hack](https://www.nowsecure.com/blog/2013/01/14/exploiting-printers-via-jetdirect-vulnerabilities/)

[惠普pjl/pcl资料](http://h20566.www2.hp.com/hpsc/doc/public/display?docId=emr_na-bpl13207)


* UEL (Universal Exit Language ) 
  - Command used at the beginning and end of each data stream sent to the printer. The syntax is represented as %-12345X , where ESCape can be represented by 0x1B.
* PJL (Printer Job Language )
  - Used to tell the printer what action to take. It serves as additional support for the PCL.
* PCL (Printer Control Language )
  - Basically a language used to format pages. Although it may seem harmless, it can be used to exploit vulnerabilities in most parsers and interpreters.