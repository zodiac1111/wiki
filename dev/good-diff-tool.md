# 好用的diff工具

##meld

[meld](http://meldmerge.org/) gui图形化 本比较目录及所有子文件/目录

将meld设置为git的默认diff tool:

http://blog.csdn.net/offbye/article/details/6592563

## gvim

`gvim` gui图形化 比较文件

## git(+其他)

原始的 `git diff` 文本

可以通过设置`difftool`调用其他的diff工具,`git difftool --help` 查看详情

支持一下工具:

```
-t <tool>, --tool=<tool>
           Use the diff tool specified by <tool>. Valid diff tools are: araxis, bc3, deltawalker, diffuse, emerge,
           ecmerge, gvimdiff, kdiff3, kompare, meld, opendiff, p4merge, tkdiff, vimdiff and xxdiff.

```