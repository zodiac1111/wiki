# shadowsocks

# 介绍
# 原理
# 使用
## 客户端

### 软件安装

```bash
sudo apt-get install shadowsocks
```

默认配置文件:`/etc/shadowsocks/config.json`

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

修改配置ip和密码

运行

```bash
sslocal -c /etc/shadowsocks/config.json
```

## 服务端

```bash
sudo apt-get install shadowsocks
```

修改IP(不知道是否必要)和密码

启动
```bash
/etc/init.d/shadowsocks start
```