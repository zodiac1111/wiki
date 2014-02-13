# linux时间

> 鸟哥时间 http://linux.vbird.org/linux_server/0440ntp.php 

# 软件时间(系统时间)

##设置时区

```bash
echo GMT0BST > /etc/TZ
```

## 通过时间戳设置时间

```bash
date +s @<时间戳>
```
比如:
```bash
date +s @0 # 原点
```
## 设置时间

    date -s "20070414 13:58:00" 
    date -s "2007-04-14 13:58:00" (webbox通过,可行)
    date -s "2007/04/14 13:58:00" 

## 以时间戳形式查看时间
```
date +%s
```
## 查看时区
```bash
date -R
```

# 硬件时间
```bash
将系统时间(date)写入硬件时间
hwclock -w
```