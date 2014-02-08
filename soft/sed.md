# sed 使用

 -e <表达式> 执行(多个)表达式

sed -e '1,5d' -e 's/test/check/' example   

 -n 只显示匹配的行

 -i 插入,修改原来的文件而不是输出到标准输出

多个单词匹配`\(static\|dhcp\)`

http://stackoverflow.com/questions/1251999/sed-how-can-i-replace-a-newline-n/1252191#1252191

```
sed  -e '/^.*#/d'  \
     -e '0,/^$/d' \
     -e '/^.*iface eth0.*$/{:a;N;$!ba;s/netmask/AAA/}' \
     interfaces
```