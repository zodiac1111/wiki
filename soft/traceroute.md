# traceroute

路由追踪

```bash
sudo traceroute baidu.com
```

看到很多星号是服务不返回信息,试试其他各种选项

```
sudo traceroute baidu.com --icmp
sudo traceroute baidu.com --tcp
sudo traceroute baidu.com --udp
```

或者本机连接国外vpn,再通过vpn跳转