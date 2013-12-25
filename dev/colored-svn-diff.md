# 对svn diff着色

# 安装专门的 colordiff 软件

http://linux-hints.blogspot.com/2011/09/colored-svn-diff.html

Install colordiff (eg. on Ubuntu):
```
sudo apt-get install colordiff
```
Simply redirect svn diff output to colordiff:
```
svn diff -r Rev1:Rev2 file | colordiff
```
You can declare a simple function in your ~/.bashrc file:
```
svndiff() {
svn diff "${@}" | colordiff 
}
```

# 传递给其他程序处理,比如vim

http://www.commandlinefu.com/commands/view/2420/colored-svn-diff

```
svn diff <file> | vim -R -
```

```
svn diff | view -
```
