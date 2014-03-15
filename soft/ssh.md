# ssh使用
参考: 

* 鸟哥 http://linux.vbird.org/linux_server/0310telnetssh.php#ssh_server
* http://blog.sina.com.cn/s/blog_643493600100uje6.html

1.启动SSH服务
```
systemctl start sshd.service
```
2.随系统一起启动服务
```
systemctl enable sshd.service
```
3.开启防火墙22端口
```
iptables -I INPUT -p -tcp --dport 22 -j ACCEPT
```


===================================

	$ sudo systemctl start sshd.service
                              
	$ sudo systemctl enable sshd.service
	ln -s '/usr/lib/systemd/system/sshd.service' '/etc/systemd/system/multi-user.target.wants/sshd.service' 
===================================

# sshd
## 反向连接

> http://www.cnblogs.com/eshizhan/archive/2012/07/16/2592902.html

* 外网 <OUT_IP>
* 内网 <IN_IP> 运行sshd

内网运行
```
ssh -f -N -R <本地监听的端口>:localhost:<本地sshd端口> root@vps2 [-p<vps2的sshd的端口]
ssh -f -N -R <本地监听的端口>:localhost:22 root@vps2 [-p<vps2的sshd的端口]
```

图形
```
ssh -X 主机 [-p端口] <指令>
```