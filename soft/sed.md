# sed 使用

```
 sed -e '1,5d' -e 's/test/check/' example   
   -e <表达式> # 执行(多个)表达式
   -n 只显示匹配的行
   -i 插入,修改原来的文件而不是输出到标准输出
```

多个单词匹配`\(static\|dhcp\)`

http://stackoverflow.com/questions/1251999/sed-how-can-i-replace-a-newline-n/1252191#1252191

```
sed  -e '/^.*#/d'  \
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