# pptpd 服务器,安装配置


安装服务器教程

1. http://www.ebayram.net/howto-vpn-pptp-server-debian-ubuntu/
2. http://www.networkinghowtos.com/howto/configure-a-pptp-vpn-server-on-ubuntu-linux/
3. 详细(包括客户端设置) http://www.howtogeek.com/51237/setting-up-a-vpn-pptp-server-on-debian/
4. windowXP 不代替默认路由(连上vpn还可以通过本机访问互联网,而不是通过vpn访问互联网) http://service.tp-link.com.cn/detail_article_414.html

其他一些客户端设定 [[pptp]] 

路由.网络操作 [[ifconfig]]

```text
# 查看状态
sudo service pptpd status 
sudo service pptpd restart #重启
#服务器配置
sudo vim /etc/pptpd.conf
#服务器选项
sudo vim /etc/ppp/pptpd-options
# 这种加密的用户管理
sudo vim /etc/ppp/chap-secrets 

```