# svn 使用笔记

# 升级问题

客户端从1.6升级到1.7的时候

参考: http://blog.chinaunix.net/uid-10769062-id-3754341.html

```bash
svn: E155036: 请参阅命令 'svn upgrade'
svn: E155036: 工作副本 '/usr/local/usr/project/video/trunk/mpp' 格式太旧 (格式 10, Subversion 1.6 创建)
```

到本地仓库根目录,`svn upgrade`.

# 服务器发送了意外的返回值(405 Method Not Allowed)

参考: http://blog.csdn.net/kevinew/article/details/6118420

服务器发送了意外的返回值(405 Method Not Allowed)，在响应 “MKCOL” 的请求
* svn (405 Method Not Allowed) 在响应 “MKCOL” 的请求

* I managed to solve the problem:
* Delete the parent’s directory of the folder giving the problem.
* Did SVN Update
* A folder with the same name as the new one already existed in repository.
* Delete this folder
* SVN Commit
* Copy the new folder, Schedule for addition and SVN Commit 

解释一下：

SVN出现这个错误的原因是我删除了一个文件夹后又创建了一个同名文件夹。在svn server端，好像是不能区分这两个文件夹，所以出现了错误。

解决方法：

* 删除出现错误的文件夹
* SVN Update
* 这时服务器上存在的文件夹会出现在本地
* 删除原有的文件夹
* SVN Commit
* 重新创建文件夹
* SVN Commit

# 忽略

参考: http://my.oschina.net/shelllife/blog/142257

```bash
$ svn propedit svn:ignore <目录>
```