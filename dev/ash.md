# ash脚本变成

主要与bash不同的地方

* wiki介绍 http://en.wikipedia.org/wiki/Almquist_shell
* wiki中文 http://zh.wikipedia.org/wiki/Almquist_shell
* 官方网站 http://www.in-ulm.de/~mascheck/various/ash/


## 数组
> https://forum.openwrt.org/viewtopic.php?id=10824
```ash
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

```ash
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