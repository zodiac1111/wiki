# 命令行下下载管理器aria2

> http://blog.mitemitreski.com/2012/01/aria2-awesome-command-line-download.html#.U8CQa3WSzrc

官方网址 http://aria2.sourceforge.net/

官方手册  http://aria2.sourceforge.net/manual/en/html/aria2c.html

# 安装

For Debian based (Ubuntu , Linux Mint):

    sudo apt-get install aria2 

Fedora and yum based

    sudo yum install aria2


启动下载

    aria2c --conf-path=/mnt/aria2.conf

# 使用

Few simple examples 

Download a file

    aria2c  location-or-URL-of-file

Download from multiple sources

    aria2c  location1 location2 location3 ... 

Download torrent

    aria2c location-of-file.torrent 

List files in torrent

    aria2c -S location-of-file.torrent 

Download specific files from torrent

    aria2c --select-file=1,2,3... location-of-file.torrent

index of files ( 1,2,3 ) can be  found with -S command
Limit speed for upload ( on torrent files for example )

    aria2c --max-upload-limit=200K location-of-file.torrent 

 
Limit speed for download

    aria2c --max-download-limit=100K location-of-file

 
Pause and resume
In order to pause just press Ctrl+C and to resume enter the same command at the same location. Aria2 keeps .aria2 files where it memorizes the current download progress. 
Download using multiple connections

    aria2c -j7 -x2 location-of-file  other-location-of-file...


this way we instruct aria2c to use 7 concurrent connections with max 2 connections per server
Download  a list of files from URL provided in a file

    aria2c -i file-with-urls.txt 

# 示例配置文件

默认使用 `~/.aria2/aria2.conf` 作为配置文件

以下指令可以设置使用的配置文件

   aria2c --conf-path=<PATH>

一个简单的配置文件例子和说明

```text
# 启动时使用下面选项可以指定配置文件  
# --conf-path=<PATH>


#允许rpc
enable-rpc=true

#允许非外部访问
rpc-listen-all=true

#允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true

# 保存的路径
dir=/mnt/aria2

#保存会话
save-session=/mnt/session 

# 重新启动时加载会话
input-file=/mnt/session

# 自动保存会话
--save-session-interval=60

#断点续传
continue=true

#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5

#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
#min-split-size=10M
#单文件最大线程数, 路由建议值: 5
#split=5
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
```