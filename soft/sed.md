# sed 使用

[sed](https://zh.wikipedia.org/wiki/Sed)既流编辑器(stream editor).

```
 sed -e '1,5d' -e 's/test/check/' example   
   -e <表达式> # 执行(多个)表达式
   -n 只显示匹配的行
   -i 插入,修改原来的文件而不是输出到标准输出
```

正则表达式匹配多个**单词**

    \(static\|dhcp\)

既匹配`static`和`dhcp`

参考:[stackoverflow](http://stackoverflow.com/questions/1251999/sed-how-can-i-replace-a-newline-n/1252191#1252191)


```
sed  -e '/^.*#/d' \
     -e '0,/^$/d' \
     -e '/^.*iface eth0.*$/{:a;N;$!ba;s/netmask/AAA/}' \
     interfaces
```

# 例子

```bash
# 修改dhcp还是静态
sed  -e '/^[\v\t\ ]*#/d' -e '/^.*iface eth0.*$/{s/\(static\|dhcp\)/dhcp/}' interfaces
# 匹配行到结尾查找 修改子网掩码
sed  -e '/^[\v\t\ ]*#/d' \
-e '/^.*iface eth1.*$/{:a;N;$!ba;s/netmask[0-9. ]*$/netmask 192.168.2.1/}' interfaces
```