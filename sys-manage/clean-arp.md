# 清理arp

> http://wscyza.blog.51cto.com/898495/728717

系统初始arp环境
```
arp -n
```

执行清除所有arp 缓存命令
```
arp -n|awk '/^[1-9]/{print "arp -d  " $1}'|sh -x
```

清理某一个ip的mac/arp
```
arp -d 192.168.1.2
```