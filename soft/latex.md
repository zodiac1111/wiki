# LaTex使用

[[http://cdn.sstatic.net/tex/img/logo.png]]

ex爆栈网的Tex专题 [tex.stackexchange.com](http://tex.stackexchange.com/)


# 页眉页脚

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

开关某些章节,显示章节级数 http://blog.sina.com.cn/s/blog_5e16f1770100gvje.html

http://hi.baidu.com/southhill/item/06c4ddd2b4f144e3b2f7776f

目录pdf 超链接 http://blog.sina.com.cn/s/blog_5e16f1770100fkcz.html


关闭目录编号 http://www.tuicool.com/articles/bMV3U3

```text
更改章节编号的格式： 
\renewcommand\thesection{\arabic{section}.} 
\renewcommand\thesection{\arabic{section}.~} 
\renewcommand\thesection{\arabic{section}.\kern -.5em}

\section{chapter} 
\subsection{sub1} 
\subsection{sub2}

\section*{Appendix} 
\subsection{Appendix1} 
\subsection{Appendix2}

\section*不显示章节编号，但是有两个问题：1）章节编号会计数，后面的章节编号会增加；2）不会编入目录中

对于问题一可以用这一行重新开始计数： 
\setcounter{subsection}{0} 
\setcounter{subsection}{1}

对于问题二，如果不想显示章节编号，又想要目录项，可关闭编号： 
\setcounter{secnumdepth}{-1}
```

# 封面设计

http://zh.wikibooks.org/zh/LaTeX/%E7%94%9F%E6%88%90%E5%B0%81%E9%9D%A2%E5%92%8C%E6%A0%87%E9%A2%98

http://www.latexstudio.net/latex-make-cover-resource-sharing/

官方例子(一般) http://ftp.ctex.org/mirrors/CTAN/info/latex-samples/TitlePages/titlepages.pdf

# 页面边距

一个页面的设置 http://blog.sina.com.cn/s/blog_531bb7630101832g.html

# 字体大小

http://blog.sina.com.cn/s/blog_5caa94a0010106ut.html
http://blog.csdn.net/xiazdong/article/details/8892070
```text
\fontsize{字号}{行距}
这个命令对其后所有文本都起作用，在使用此命令后需要用 \selectfont 才能使字体大小设置起作用。

我们通常会遇到别人规定：“正文用小四，宋体”，但是 LaTeX 并没有小四，只有 pt，因此下表为字号对应的转换表：

字号	 初号	 小初	 一号	 小一	 二号	 小二	 三号	 小三	 四号	 小四	 五号	 小五	 六号	 小六	 七号	 小七
大小	 42pt	 36pt	 26pt	 24pt	 22pt	 18pt	 16pt	 15pt	 14pt	 12pt	 10.5pt	 9pt	 7.5pt	 6.5pt	 5.5pt	 5pt
1.5行距时的 \baselineskip 设置值	 63pt	 54pt	 39pt	 36pt	 33pt	 27pt	 24pt	 22.5pt	 21pt	 18pt	 15.75pt	 13.5	 11.25	 9.75pt	 8.25pt	 7.5pt
```

# 日期

中文日期 http://blog.sina.com.cn/s/blog_5e16f1770100fve1.html

# 表格

图,墙 http://en.wikibooks.org/wiki/LaTeX/Tables

# 图片


# 一些错误

## 错误1

类似这样的提示

```text
grep: pzdr.log: 没有那个文件或目录
mktextfm: `mf-nowin -progname=mf \mode:=ljfour; mag:=1; nonstopmode; input pzdr' failed to make pzdr.tfm.
kpathsea: Appending font creation commands to missfont.log.

! Font \XeTeXLink@font=pzdr at 0.00002pt not loadable: Metric (TFM) file or ins
talled font not found.
<to be read again> 
                   \relax 
l.5125   \font\XeTeXLink@font=pzdr at 1sp\relax
                                               
? ^C! Interruption.
l.5125   \font\XeTeXLink@font=pzdr at 1sp\relax
                                               
? 
```


试试这个

参考 http://tex.stackexchange.com/questions/50596/latex-font-error-i-cant-find-file-pzdr
```bash
sudo apt-get install texlive-fonts-recommended
```

