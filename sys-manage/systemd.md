# systemd


## systemd 自定义例子

```
# @file aaa.service
# @local /etc/systemd/system/aaa.service
[Unit]
# 服务名称
Description=aaa service
After=network.target

[Service]
# 环境变量
Environment=A=123
# 启动程序
ExecStart=top
User=root
# The configuration file application.properties should be here:
#change this to your workspace
WorkingDirectory=/root
#path to executable. 
#executable is a bash script which calls jar fileExecStart=/home/ubuntu/workspace/my-webapp
SuccessExitStatus=143
TimeoutStopSec=10
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
```