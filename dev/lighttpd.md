# lighttpd 笔记

参考

1. http://www.cyberciti.biz/faq/howto-lighttpd-add-pcre-support/
2. http://www.ridgesolutions.ie/index.php/2013/06/05/build-cross-compile-lighttpd-for-arm-linux-with-pcre/
3. http://www.cyberciti.biz/tips/installing-and-configuring-lighttpd-webserver-howto.html
4. 中文 http://blog.163.com/ljf_gzhu/blog/static/131553440201212852848584
5. 编译 中文 http://blog.sina.com.cn/s/blog_6277d6450100ka5u.html
6. c语言 cgi 
  * http://blog.csdn.net/lanmanck/article/details/5355451

# 编译

## pcre

首先需要pcre

主要参考1


    wget -c ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.34.zip # 下载


    unzip pcre-8.34.zip #解压


    cd pcre-8.34/


    ./configure --host=arm-linux CC=arm-linux-gcc AR=arm-linux-ar \
     STRIP=arm-linux-strip RANLIB=arm-linux-ranlib \
    --prefix='/home/zodiac1111/Downloads/lighttpd-1.4.33/install'

## lighttpd

配置

    ./configure --host=arm-linux CC=arm-linux-gcc AR=arm-linux-ar STRIP=arm-linux-strip RANLIB=arm-linux-ranlib \
    --prefix='/home/zodiac1111/Downloads/lighttpd-1.4.33/install' \
    PCRECONFIG='/home/zodiac1111/Downloads/lighttpd-1.4.33/install/bin/pcre-config' \
    PCRE_LIB='/home/zodiac1111/Downloads/lighttpd-1.4.33/install/lib/libpcre.a' \
    CFLAGS="$CFLAGS -DHAVE_PCRE_H=1 -DHAVE_LIBPCRE=1 -I/home/zodiac1111/Downloads/lighttpd-1.4.33/install/include"

编译

    make && make install


# 使用

复制 `lighttpd`和`libpcre.so`文件到指定目录,并做好符号连接

复制`tests`(前端代码)目录到合适位置.

运行

    lighttpd -f /etc/lighttpd.conf -m /data/lib/

显示

    2014-03-25 16:17:02: (log.c.166) server started

则完成

## 配置文件

最简单的`lighttpd.conf`配置文件:

```
# 前端根目录,必须
server.document-root = "/data/var/www"

#server.port = 8080 #默认80
#server.username = "lighttpd" #默认运行的用户
#server.groupname = "lighttpd" #不指定则运行的用户组
#server.bind  = "127.0.0.1" # 不指定则都监听
#server.tag ="lighttpd" #可以不指定

#server.errorlog            = "/var/log/lighttpd/error.log" #不指定则不记录
#accesslog.filename         = "/var/log/lighttpd/access.log" #不指定则不记录

server.modules              = (
                            "mod_access",
                            "mod_accesslog",
                            "mod_fastcgi",
                            "mod_rewrite",
                            "mod_auth"
                           )
server.modules +=("mod_cgi") # 使用c语言编写cgi程序的话,这里必须

# mimetype mapping  这一系列待定
mimetype.assign             = (
  ".pdf"          =>      "application/pdf",
  ".sig"          =>      "application/pgp-signature",
  ".spl"          =>      "application/futuresplash",
  ".class"        =>      "application/octet-stream",
  ".ps"           =>      "application/postscript")

```

## 使能压缩

参考
* 英文,简单 http://www.cyberciti.biz/tips/lighttpd-mod_compress-gzip-compression-tutorial.html
* 中文,简要 http://www.deepvps.com/config-lighttpd-with-gzip-and-cache.html
* 官方英文详细 http://redmine.lighttpd.net/projects/lighttpd/wiki/Docs_ModCompress

注意点:

* `server.modules += ( "mod_compress" )` 压缩模块
* `compress.cache-dir = "/tmp/lighttpdcompress/"` cache保存路径
* `compress.filetype           = ("text/plain","text/css", "text/xml", "text/javascript" )` 压缩的文件类型