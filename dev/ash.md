# ash脚本变成

主要与bash不同的地方

* wiki介绍 http://en.wikipedia.org/wiki/Almquist_shell
* wiki中文 http://zh.wikipedia.org/wiki/Almquist_shell
* man手册 http://minix1.woodhull.com/manpages/man1/ash.1.html
* 官方网站 http://www.in-ulm.de/~mascheck/various/ash/


## 数组
> https://forum.openwrt.org/viewtopic.php?id=10824

简单示例

```sh
#!/bin/ash

ARRAY=" \
http://openwrt.org \
http://wiki.openwrt.org \
"
i=1

for j in $ARRAY
do
        echo $i: $j
        i=`expr $i + 1`
done
```
## 函数

简单示例

```sh
mk_modules_json()
{
	file=$1
	echo "$0 in function"
	echo -en '{\n' >> $file
	# wifi,目前只能有效地识别出是不是wifi
	lsusb |grep '0bda:8179'
	if [ $? -eq 0 ];then
		echo -e '"gprs":false' >> $file
		echo -e ',"wifi":true' >> $file
		echo -e ',"3g":false' >> $file
		echo -en '}' >> $file
	else
	# 其他就先统一识别成为 gprs
		echo -e '"gprs":true' >> $file
		echo -e ',"wifi":false' >> $file
		echo -e ',"3g":false' >> $file
		echo -en '}' >> $file
	fi
}
```

### 函数的参数列表

* http://stackoverflow.com/questions/16461656/bash-how-to-pass-array-as-an-argument-to-a-function
* bash的传递参数例子 http://bash.cyberciti.biz/guide/Pass_arguments_into_a_function

这个例子将所有参数列表合在一起,使用`$@`.
```sh
isConnect()
{	
	local servers=$@
	echo "所有服务器:"$servers
	local i=1 ;
	local j=0 ;
	for j in $servers
	do
		    echo $i: $j
		    i=`expr $i + 1`
	done
	return 2  ;
}
```