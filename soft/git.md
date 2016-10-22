##git diff --color /auto
在shell中 :

### auto
`$ git config --global color.diff auto`
### always
`$ git config --global color.diff always`

then,in the file `~/.gitconfig`(becase of the `--global` option)

```
[color]
	diff = auto     # diff color =auto :)
```

扩展

```
[color]
	diff = auto
	log = auto
	branch = auto
	ui = true	#设置用户界面都使用色彩高亮
```

##git 配置文件 
1. `/etc/gitconfig`
2. `~/.gitconfig`
3. `项目下/gitconfig`

三者作用域:所有用户/当前用户/当前用户的指定项目
且作用域小的覆盖作用域大的.(与c语言变量作用域相似)

## git 小修改/隐藏/应用/tips

`git status -uno` 显示未跟踪的文件


```
git stash `#隐藏当前修改
git stash apply `#显示当前修改
```
用于临时修改其他与现在没有逻辑相关性的修改.

###git merge /rebase

情况1 merge:(主分支没有更新)

切换到主分支,合并子分支到主分支

`git merge path1`

情况2 rebase:(主分支在子分支 分出之后 又**更新**了)

所以,组员在修改自己的东西前最好先checkout自己的branch,而不是直接修改主分支master

## 强制获取远端

放弃本地修改,强制从服务器获取最新版本.所有本地修改将被删除!

```
git fetch --all  
git reset --hard origin/master 
```

参考:https://ruby-china.org/topics/2494

# 自建远程仓库

远程主机:
```
mkdir test.git
cd test.git
git init --bare
```

## 全新仓库

本地主机
```
git clone (user)@(remote host):/path/test.git
```

## 本地既存

本地主机
```
git remote add (ropename) (user)@(remote host):/path/test.git
```


#参考

1. `man git-config`
2. `man git`
