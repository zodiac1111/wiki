# 输入/输出错误 

linux下文件系统,mac下挂载过以后可能出现的 无法rm ls 等

使用 fsck检查一下磁盘

先umount磁盘

找到磁盘的设备文件,比如这里的`sdc`

`fsck`检查磁盘

`sudo fsck /dev/sdc`


```
fd3308d/.Trashes' zodiac1111/note# rm -rf '/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5 
fd3308d/.fseventsd' diac1111/note# rm -rf '/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5 
08d/.Trash-1000/files/itunes' ote# cd '/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd33 
s# lldebian:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files/itunes
ls: 无法访问东邪西毒: 输入/输出错误
总用量 0
d????????? ? ? ? ?             ? 东邪西毒
s# cd ..
root@debian:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files# ls
itunes
tunes/ -ran:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files# rm i 
rm: 无法删除"itunes/东邪西毒": 输入/输出错误
root@debian:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files# 
root@debian:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files# 
root@debian:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files# 
root@debian:/media/zodiac1111/bcb95e11-a242-494e-bd4c-b88f5fd3308d/.Trash-1000/files# exit
zodiac1111@debian:note$ sudo fsck /dev/sdc
[sudo] password for zodiac1111: 
fsck，来自 util-linux 2.25.2
e2fsck 1.42.12 (29-Aug-2014)
/dev/sdc contains a file system with errors, 强制检查.
第一步: 检查inode,块,和大小
第二步: 检查目录结构
在 /.Trash-1000/files/itunes (55050241) 中的入口 'M-dM-8M-^\M-iM-^BM-*M-hM-%M-?M-fM-/M-^R' has 删除/unused inode 55051877.  清除<y>? 是
第3步: 检查目录连接性
/lost+found未找到.创建<y>? 否
Pass 3A: Optimizing directories
Pass 4: Checking reference counts
第5步: 检查簇概要信息

/dev/sdc: ***** 文件系统已修改 *****

/dev/sdc: ********** 警告: 文件系统上仍有错误 **********

/dev/sdc: 1754485/61054976 files (0.3% non-contiguous), 192147780/244190646 blocks

```