# boa cgi 笔记

# cgi 502错误

## 故障表现

```
[02/Jan/2007:22:00:38 +0000] cgi_header: unable to find LFLF
```
```
<HTML><HEAD><TITLE>502 Bad Gateway</TITLE></HEAD>
<BODY><H1>502 Bad Gateway</H1>
The CGI was not CGI/1.1 compliant.
</BODY></HTML>
```

## 故障排除

### 不能执行

确认cgi可以执行: 在target上执行cgi程序,比如 1.cgi.

如果不能执行.则.比如需要`chmod +x`等.

### boa允许

默认boa配置文件`/etc/boa/boa.conf`使能boa执行cgi程序

```
# Uncomment the next line if you want .cgi files to execute from anywhere   
AddType application/x-httpd-cgi cgi  
```

### 进程通讯错误

比如共享内存读取,但是共享内存并未被其他进程创建.
