# awk 简单使用笔记

参考:
* http://coolshell.cn/articles/9070.html

# 最简单用法 

打印

    awk '{print $1, $4}' in.txt

# 不要打印换行

参考 http://stackoverflow.com/questions/2021982/awk-without-printing-newline

`print`-> `printf`

# 自定义分隔符

    awk -F, 'program' input-files

