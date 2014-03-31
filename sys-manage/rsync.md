# 使用rsync同步/备份文件

> Linux远程备份工具Rsync使用案例 http://os.51cto.com/art/201009/225962.htm

参考 : http://www.thegeekstuff.com/2010/09/rsync-command-examples/

# 忽略

> http://ubuntuforums.org/showthread.php?t=1727787

同步文件夹,但忽略点(.)开始的文件(隐藏文件)

使用 `--exclude` 选项

    rsync -av  --exclude=".*" /src/ /dst/ -r

# alias

仿造scp,使用rsync同步差异

    alias rscp='rsync -v -P -e ssh'