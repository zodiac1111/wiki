# lighttpd 笔记

参考

1. http://www.cyberciti.biz/faq/howto-lighttpd-add-pcre-support/
2. http://www.ridgesolutions.ie/index.php/2013/06/05/build-cross-compile-lighttpd-for-arm-linux-with-pcre/

# 编译

## pcre

首先需要pcre

主要参考1

```
wget -c ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.34.zip # 下载
unzip pcre-8.34.zip #解压
cd pcre-8.34/
```