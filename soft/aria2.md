# 命令行下下载管理器aria2

> http://blog.mitemitreski.com/2012/01/aria2-awesome-command-line-download.html#.U8CQa3WSzrc

# 安装

For Debian based (Ubuntu , Linux Mint):

    sudo apt-get install aria2 

Fedora and yum based

    sudo yum install aria2

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

    aria2c --max-download-limit=100K location of file

 
Pause and resume
In order to pause just press Ctrl+C and to resume enter the same command at the same location. Aria2 keeps .aria2 files where it memorizes the current download progress. 
Download using multiple connections

    aria2c -j7 -x2 location-of-file  other-location-of-file...


this way we instruct aria2c to use 7 concurrent connections with max 2 connections per server
Download  a list of files from URL provided in a file

    aria2c -i file-with-urls.txt 