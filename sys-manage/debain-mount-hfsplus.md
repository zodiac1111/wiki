# debian访问osx

key: mac osx debian linux hfsplus mount

挂载选项

    force,rw,nosuid,nodev,relatime,umask=22,uid=501,gid=80,nls=utf8

linux下的磁盘工具.

* 文件类型 hfsplus
* 不选"显示用户界面"
* uid501是mac上普通用户的用户id
* gid80是mac上普通用户的组id

gui 磁盘管理器(Disk Manager)也可以编辑挂载信息 http://flomertens.free.fr/disk-manager/

挂载osx系统盘如果以uid=0 gid=0( **root** )挂载须小心,不要删除东西

* /Applications 是应用程序目录,用户安装的
* /Users 类似linux家目录 /home
