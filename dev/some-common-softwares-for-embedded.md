# 常见嵌入式基础软件

# 启动引导程序
* u-boot 德国.

# 系统库
* uclibc 小型libc(c语言库)实现
* newlibc 另一种c语言标准库实现,红帽主导?

# 基础文本
* ncurses c实现的文本窗体图形化库.
 * make menuconfig 界面就需要这个
* readline 命令行数据数据的库
 * bash输入就是依靠它实现.

# 程序语言

* lua lua语言解释器.由于使用ANIS C标准写成,非常适合移植

# 网络

* curl http://curl.haxx.se/download.html
* wget 下载的

# ssh 相关
* openssl 
 * http://www.openssl.org/source/ 
 * [编译参考](http://blog.sina.com.cn/s/blog_4ccac7230101ncyr.html)
 * http://blog.chinaunix.net/uid-20680966-id-3232074.html [原]交叉编译openssl不修改Makefile的方法 
* openssh [编译参考](http://cubietech.com/forum.php?mod=viewthread&tid=54)
* dropbear 简化的sshd(ssh服务器)

# 安全相关
* gnupg/gpg 签名认证.验证文件.防止/识别篡改

# 软件管理
* opkg 包管理软件,从ipkg演化而来

