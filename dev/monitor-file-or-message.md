# 监视文件内容或者系统消息

* 来源: http://stackroulette.com/superuser/289239/is-it-possible-to-tail-f-the-output-of-dmesg

监视最近dmesg消息
```
dmesg | tail -f
```

这个消息保存在日志文件,所以下面方式也行
```
tail -f /var/log/{messages,kernel,dmesg,syslog}
```

proc文件系统的话:`man dmesg` ,`man proc`
```
cat /proc/kmsg
```

或者使用`watch`监视
```
watch 'dmesg | tail -50'
```