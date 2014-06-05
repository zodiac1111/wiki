# LaTex使用

页眉页脚: http://blog.sina.com.cn/s/blog_9908653401010lx6.html

# 中文


中文 xelatex

## 简单的方式

**段落显示有问题,很难看**

使用字体
```latex
\documentclass{article} 
\usepackage{fontspec}
\setmainfont{STHeiti J} % 见字体名称来源
\begin{document} 
你好
\end{document} 
```



## ctex

已经被合并?

> xeCJK 的原始作者是孙文昌，2009 年 5 月起宏包被收入 ctex-kit 项目进行维护，目前主要维护者是刘海洋1 和李清2。

ctex 宏包说明 :

http://linux-wiki.cn/wiki/zh-hans/LaTeX%E4%B8%AD%E6%96%87%E6%8E%92%E7%89%88%EF%BC%88%E4%BD%BF%E7%94%A8XeTeX%EF%BC%89

```latex
\documentclass{article}
\usepackage{ctex}
\begin{document}
中文宏包测试
\end{document}
```

或者

```latex
\documentclass{ctexart}
\begin{document}
中文宏包测试
\end{document}
```
### 问题

```text
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!
! fontspec error: "font-not-found"
! 
! The font "SimSun" cannot be found.
! 
! See the fontspec documentation for further information.
! 
! For immediate help type H <return>.
!...............................................  
                                                  
l.5   {SimSun}
```

**没有解决**

## xeJCK

目前正在使用!

宏包说明: http://ctan.mirrorcatalogs.com/macros/xetex/latex/xecjk/xeCJK.pdf

参考:
http://tex.stackexchange.com/questions/160666/how-can-i-install-a-chinese-font-in-a-ubuntu-13-10-in-a-way-that-xelatex-can-see

```latex
\documentclass{article} 
% 中文库和字体设置
\usepackage{xeCJK}
\setCJKmainfont{WenQuanYi Zen Hei} % 见字体名称来源
\begin{document} 
```

## 字体名称来源

其中字体描述信息为
```bash
fc-list :lang=zh
或者
fc-list
```
命令输出的
```bash
/home/zodiac1111/.fonts/STHeiti-Light.ttc: 黑体\-韩语,黑體\-韓語,Heiti K,黒体\-韓国語,Heiti\-한국어:style=细体,細體,Mager,Fein,Light,Ohut,Fin,Leggero,ライト,가는체,Licht,Tynn,Leve,Светлый,Fina
/usr/share/fonts/truetype/arphic/uming.ttc: AR PL UMing CN:style=Light
/usr/share/fonts/truetype/arphic/uming.ttc: AR PL UMing HK:style=Light
/usr/share/fonts/truetype/wqy/wqy-microhei.ttc: 文泉驿等宽微米黑,文泉驛等寬微米黑,WenQuanYi Micro Hei Mono:style=Regular
/home/zodiac1111/.fonts/STHeiti-Medium1.ttc: 黑体\-日本语,Heiti J:style=中等,Medium
/home/zodiac1111/.fonts/STHeiti-Light.ttc: 黑体\-简,黑體\-簡,Heiti SC,黒体\-簡,Heiti\-간체:style=细体,細體,Mager,Fein,Light,Ohut,Fin,Leggero,ライト,가는체,Licht,Tynn,Leve,Светлый,Fina
/home/zodiac1111/.fonts/STHeiti-Medium2.ttc: STHeiti T0C:style=Medium
/usr/share/fonts/zh_CN/simsum.ttf: 方正魏碑_GBK,FZWeiBei\-S03:style=Regular
/usr/share/fonts/wps-office/FZYTK.TTF: 方正姚体_GBK,FZYaoTi\-M06:style=Regular
/usr/share/fonts/wps-office/FZSSK.TTF: 方正书宋_GBK,FZShuSong\-Z01:style=Regular
```

格式:
```bash
<路径>: 中文名,英文名:style=<样式>
```

其中的英文名部分 ,如
```text
FZShuSong\-Z01
```
但是dash(-) 不需要反斜杠转义.

# 目录标题

http://hi.baidu.com/southhill/item/06c4ddd2b4f144e3b2f7776f

# 封面设计

http://www.latexstudio.net/latex-make-cover-resource-sharing/
