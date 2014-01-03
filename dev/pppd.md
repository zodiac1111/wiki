# pppd 笔记

启动脚本

```
pppd call <脚本名称>
pppd call tdscdma &
```
脚本内容
```
# cat tdscdma
nodetach
lock
/dev/ttyS4
115200
user "cmnet"
crtscts
modem
hide-password
usepeerdns
noauth
noipdefault
novj
debug
novjccomp
noccp
defaultroute
ipcp-accept-local
ipcp-accept-remote
connect 'chat -s -v -f /etc/ppp/peers/chat-tdscdma-connect'
disconnect 'chat -s -v -f /etc/ppp/peers/chat-tdscdma-disconnect'
```