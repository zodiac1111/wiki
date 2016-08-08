# sdr
工具: linux下类似 sdr# (srdsharp)

# 调频收音机

使用方法,比如 收音机,fm93

https://yinlei.org/x-plane10/2014/11/gqrx-2-3-1.html

```
gqrx
```

运行

一些使用技巧 
http://beryl.at/gqrx_on_osx_and_adsb_signal/


Mode 这里我们可以选择信号类型，
一般就是 WFM（我们普通听的电台），
Narrow FM（窄带 FM，一般出租车司机们用的 Call 台，以及一些公司、保安用的对讲机），
AM（短波，民用无线电，航空管制等）。
AGC 选 Medium 或者 Fast 即可。Squelch 一会儿真正听的时候再调。


RTL - 便宜的类型 起源,来源:http://rtlsdr.org/#history_and_discovery_of_rtlsdr
相比开源SDR中的另外两位重量级全功能（带收发,FPGA）选手：动辄几K美刀的USRP、虽说便宜但是也一二百美刀的HackRF，RTL以其实用的功能、相当于白送的价格，以及几乎完全没有法律风险，成为了入门或者廉价SDR的代名词。


MCX 接口 SMA接口 天线接口 ; 淘宝图片: https://s.taobao.com/search?q=MCX%E5%85%AC++SMA%E6%AF%8D&imgfile=&js=1&stats_click=search_radio_all%3A1&initiative_id=staobaoz_20160522&ie=utf8&loc=%E6%B1%9F%E8%8B%8F%2C%E6%B5%99%E6%B1%9F%2C%E4%B8%8A%E6%B5%B7

# 嗅探gprs

hackrf and kaliOS, need gr-gsm ( Python install or Ubnutu PPA)

https://z4ziggy.wordpress.com/2015/05/17/sniffing-gsm-traffic-with-hackrf/

chinese version: http://www.bkjia.com/aqgjrj/1014112.html

Issue SSLv3:

https://github.com/ptrkrysik/gr-gsm/issues/155

# OpenBTS debug the GSM/GPRS
Chinese version : http://www.freebuf.com/articles/wireless/105324.html
Via(English): https://www.nccgroup.trust/uk/about-us/newsroom-and-events/blogs/2016/may/gsmgprs-traffic-interception-for-penetration-testing-engagements/
gsm freq
http://www.worldtimezone.com/gsm.html

# 使用GnuRadio + OpenLTE + SDR 搭建4G LTE 基站（上）
http://www.freebuf.com/articles/wireless/108417.html

OpenLTE : 4G LTE

ref: http://www.rtl-sdr.com/rtl-sdr-tutorial-analyzing-gsm-with-airprobe-and-wireshark/

# GNURadio Live DVD
http://gnuradio.org/redmine/projects/gnuradio/wiki/GNURadioLiveDVD

# gr-gsm
https://github.com/ptrkrysik/gr-gsm/wiki/Installation

## gsm scaner

```
root@tergHTEm:~# grgsm_scanner 
linux; GNU C++ version 5.3.1 20160323; Boost_105800; UHD_003.010.000.git-249-gef57ffcb

ARFCN:    3, Freq:  935.6M, CID: 24580, LAC: 22546, MCC: 460, MNC:   0, Pwr: -50
```

or

```
# airprobe_rtlsdr_scanner.py 
linux; GNU C++ version 5.3.1 20160323; Boost_105800; UHD_003.010.000.git-249-gef57ffcb

ARFCN:    3, Freq:  935.6M, CID: 24580, LAC: 22546, MCC: 460, MNC:   0, Pwr: -49
```


## linsten

```
airprobe_rtlsdr.py
```

## see it in wireshark

```
sudo wireshark -k -Y 'gsmtap && !icmp' -i lo
```

# 嗅探gprs

参考 http://www.rtl-sdr.com/rtl-sdr-tutorial-analyzing-gsm-with-airprobe-and-wireshark/
