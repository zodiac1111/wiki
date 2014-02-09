# rtorrent 文本界面的bt客户端

# 下载安装
redhat.fedora.centos属于epel仓库 

http://pkgs.org/centos-6/epel-i386/rtorrent-0.8.6-4.el6.i686.rpm.html

安装` yum install rtorrent -y`即可.比较老.可能不支持磁力连接

这可仓库软件新一点 

http://pkgs.org/centos-6/repoforge-i386/rtorrent-0.8.9-1.el6.rf.i686.rpm.html

但是软件会有错误 `rtorrent: symbol lookup error: rtorrent: undefined symbol: _ZN7torrent10ThreadBase8m_globalE`

降级 `yum downgrade libtorrent` 即可,参考  https://www.centos.org/forums/viewtopic.php?t=4386#p21301


# 使用

> * https://wiki.archlinux.org/index.php/RTorrent_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
> * http://wangyan.org/blog/rtorrent-and-rutorrent-tutorial.html
如果在文本界面使用最好配合`screen`使用

复制配置文件

`/usr/share/doc/rtorrent-0.8.6/rtorrent.rc` 到 家目录

    cp /usr/share/doc/rtorrent-0.8.6/rtorrent.rc ~/.rtorrent.rc 

修改配置 

下载目录

directory = /home/[user]/torrents/ 使用绝对路径

保存进度

session = /home/[user]/.session/ 也是必须绝对目录

