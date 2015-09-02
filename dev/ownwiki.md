以中文文言文维基百科为例子,使用wikimedia软件在服务器/本地搭建wiki百科镜像站点.

导入wiki百科数据架设镜像站点.本地或服务器均可.** 不包含图片,图片需要非常巨大的空间**

1. 中文文件文维基百科条目较少,下载方便.
初次尝试代价较小.成功后亦可使用其他数据库.
2. 主机是Debian7/Linux操作系统
3. 使用java编写的数据库导入工具方便易用

# 安装

## web server

安装经典lamp(linux apache mysql php)集合.

	apt-get update
	apt-get install apache2 php5 libapache2-mod-php5 mysql-server mysql-client php5-mysql phpmyadmin

次过程中可能要求设置各种密码.

可选操作
* 修改apache web服务器默认端口.
* 修改mysql默认访问ip限制.

## MediaWiki

### 下载
下载MediaWiki

在[这里](https://www.mediawiki.org/wiki/Download)可以找到当前最新的`MediaWiki 1.25.2`下载链接.

下载后解压到例如`/var/www/w`路径下.

务必给予`/var/www/w/config`及其子文件可执行权限.

### 配置

通过浏览器访问 [[http://localhost/w/]] 即可看到设置页面.

根据页面上的提示输入各种信息数据库用户密码等.

之后保存配置

	cp /var/www/w/config/LocalSettings.php /var/www/w

然后访问 [[http://localhost/w/]] 即可看到一个空的wiki站点.

*至此mediawiki安装完成*

*页面可能提示基于安全原因推荐删除config文件夹,此处可以等到全部完成后再删除.*

# 导入数据

## 获得数据

维基百科的数据可以在[维基百科:数据库下载](https://zh.wikipedia.org/zh/Wikipedia:%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%8B%E8%BD%BD)页面找到.

根据提示选择一个类别,本例使用文言文版本,点击进入 [[http://download.wikipedia.com/zh_classicalwiki/]] .选择日期或者latest.

下载 zh_classicalwiki-latest-pages-articles.xml.bz2 文件

	wget https://dumps.wikimedia.org/zh_classicalwiki/latest/zh_classicalwiki-latest-pages-articles.xml.bz2

## 导入数据

参考[Data dumps/Import examples](https://meta.wikimedia.org/wiki/Data_dumps/Import_examples).

如果使用其他工具导入可能需要使用`bzip2 -d zh_classicalwiki-latest-pages-articles.xml.bz2`命令解压数据库方可导入至数据库.

**使用mwdumper.jar 不需要解压缩,但是需要安装java**

安装好java后使用以下命令导入数据至mysql数据库.

	java -jar mwdumper.jar --format=sql:1.5 \
	zh_classicalwiki-latest-pages-articles.xml | mysql -u root -p wikidb -f

* `-u root`指定数据库用户名
* `-p` 要求输入密码
* `wikidb`为默认数据名称

需要mysql相应用户的密码(密码不回显).

之后控制台类似如下显示

	root@1:~# java -jar mwdumper.jar --format=sql:1.5 zh_classicalwiki-latest-pages-articles.xml.bz2 | mysql -u root -p wikidb -f
	Enter password: 1,000 pages (645.161/sec), 1,000 revs (645.161/sec)
	2,000 pages (504.414/sec), 2,000 revs (504.414/sec)
	...
	62,530 pages (3,577.64/sec), 62,530 revs (3,577.64/sec)
	root@1:~#

**此处根据数据库大小和计算机性能可能等待多达几十小时.**所以推荐先使用条目少的百科(如:文件文)进行尝试安装.

之后再通过浏览器访问就可以看到中文文言文的条目了.

练手文言文确认无误后可以着手镜像其他维基百科了,比如中文的.

### 补充

对于非空的数据库(比如之前安装过文件文百科)参考中说明必须先清理一些表.**初次导入数据不需要做**.

使用如下语句访问mysql数据库

	mysql -u root -p

之后进入mysql的控制台.命令提示符是`mysql>`.

在`mysql`中数据以下语句使用wikidb数据库:

	mysql> use wikidb;

清理原先的表:

	mysql>DELETE FROM page; DELETE FROM text; DELETE FROM revision;

## 完善

上一步之后,文言文百科基本可用,但是还是有些模板显示有些问题,是因为缺少相应的插件造成的.

*todo安装相应插件*


# 参考

* [How to mirror Wikipedia](https://web.archive.org/web/20090124144655/http://modzer0.cs.uaf.edu/~dev2c/wiki/How_to_mirror_Wikipedia)
* [mediawiki/Download](https://www.mediawiki.org/wiki/Download)
* [Data dumps/Import examples](https://meta.wikimedia.org/wiki/Data_dumps/Import_examples)


## 实用mysql语句

安装和配置过程可能用到的mysql语句.

	mysql -p # 登陆mysql

以下是mysql控制台语句,命令提示符默认为 `mysql>`,务必注意区别.

	drop命令用于删除数据库。
	drop命令格式：drop database <数据库名>;
	create database wikidb;  // 创建一个数据库
	show databases; // 显示所有数据库
	use wikidb;	//使用wikidb数据库
	show tables;	// 显示所有表
	quit; // 退出mysql
