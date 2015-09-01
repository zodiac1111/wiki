谷歌
==========

## 知名服务

* 美国在线 http://www.aol.com/
* 美国在线 http://wow.com/

## 反向代理

* https://www.out1000.com/
* http://www.gugesou.com/
* http://www.g363.com/

## hosts

[这里](http://laod.cn/hosts/2015-google-hosts.html)

## wiki百科

* [介绍](https://meta.wikimedia.org/wiki/%E5%A6%82%E4%BD%95%E5%9C%A8%E4%B8%AD%E5%9B%BD%E5%A4%A7%E9%99%86%E8%AE%BF%E9%97%AE%E7%BB%B4%E5%9F%BA%E7%99%BE%E7%A7%91)

### 自己搭建

* 参考 [How to mirror Wikipedia](https://web.archive.org/web/20090124144655/http://modzer0.cs.uaf.edu/~dev2c/wiki/How_to_mirror_Wikipedia)

准备工作

```
apt-get update
apt-get install apache2 php5 libapache2-mod-php5 mysql-server mysql-client php5-mysql phpmyadmin 
```

此过程中会要求设置多个密码

Download MediaWiki software

[这里](https://www.mediawiki.org/wiki/Download)下载最新版

访问web页面,设置很多东西

先用文言文测试一下,数据量少 https://dumps.wikimedia.org/zh_classicalwiki/latest/

下载 zh_classicalwiki-latest-pages-articles.xml.bz2 文件

解压(用下面的导入程序不需要解压)

bzip2 -d zh_classicalwiki-latest-pages-articles.xml.bz2 

参考:[导入数据](https://meta.wikimedia.org/wiki/Data_dumps/Import_examples)

wget https://web.archive.org/web/20090124144655/http://meta.wikimedia.org/wiki/Data_dumps/mwimport

参考

用这个导入的:

https://www.mediawiki.org/wiki/Manual:MWDumper
