# emacs ide

来源: http://emacs-ide.tuxfamily.org/

[[http://emacs-ide.tuxfamily.org/screenshot.png]]

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

# 界面

分为3个窗体
```
+--------------+
|代码区 | 菜单区 |
|--------------|
|    输出区     |
+--------------+
```