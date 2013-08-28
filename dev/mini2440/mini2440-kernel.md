# 编译kernel

[目录](customize-mini2440-softwave) 

## 参考

* 使用uboot下载kernel http://my.oschina.net/fgq611/blog/101743

# 获得内核镜像

**检查点** 获得一个kernel镜像文件


### zImage->uImage

# 下载内核镜像

## 通过tftp服务.

确认以太网联通 `ping <主机IP>` 主机也能ping通目标版

uboot: 
`tftp <地址> <文件名>`