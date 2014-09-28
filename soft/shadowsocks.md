# shadowsocks

# 介绍
# 原理
# 使用
## 客户端

### 软件安装

```
sudo apt-get install shadowsocks
```

配置文件:`/etc/shadowsocks/config.json`

```json
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```

## 服务端