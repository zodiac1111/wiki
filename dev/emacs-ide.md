# emacs ide

* 来源: http://emacs-ide.tuxfamily.org/
* git仓库: http://git.tuxfamily.org/eide/emacs-ide-website.git
* github上的git镜像仓库 : https://github.com/emacsmirror/emacs-ide


# 简介

一个面向c/c++开发的ide.(eide)

* 许可: GPLv3 or later
* 语言: Emacs Lisp
* 依赖: Emacs, Ctags, Cscope.
* 系统支持: GNU/Linux

这是一个轻量级的基于emacs的ide.包含有:

1. emacs提供基础框架和编辑器
2. ctags和cscope提供标签识别
3. 编译和调试有操作系统自带的gcc和gdb等工具提供.

照日志来看,这个人从3年开始独自一人开发到现在(2013/9).很难得,目前还在更新中.

# 界面

[[http://emacs-ide.tuxfamily.org/screenshot.png]]

分为3个窗体
```
+--------------+
|代码区 | 菜单区|
|--------------|
|    输出区    |
+--------------+
```

在菜单区不选中任何文本的情况下,右击->`help`查看帮助菜单.

# 安装

* `install` 以root用户安装,或
* `install -l`以当前登录用户(非root用户)安装
* 记得按照提示在`~/.emacs`文件中添加一个函数以启动`emacs ide`.

# 使用

默认`F2`跳转到定义(ctags) `F3`跳转到定义(Cscope) .`F1`跳回.一些快捷键在`src/eide-keys.el`中定义.
