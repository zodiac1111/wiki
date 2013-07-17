# gollum wiki 使用
 
# 简介
[gollum](https://github.com/gollum/gollum)是一个wiki系统.github使用的就是gollum.可以用于个人知识管理([PKM](http://en.wikipedia.org/wiki/Personal_knowledge_management)).

特点:

* [git](http://git-scm.com/)操作,仓库
* markdown([wiki](http://en.wikipedia.org/wiki/Markdown))模块
* ruby([wiki_cn](http://zh.wikipedia.org/zh/Ruby),[wiki](http://en.wikipedia.org/wiki/Ruby_(programming_language)))语言
* web编辑(自动commit)/本地编辑器编辑(手动commit后生效)

# 安装


需要 [`ruby`](http://en.wikipedia.org/wiki/Ruby_programming_language) [`rubygems`](http://en.wikipedia.org/wiki/RubyGems) :

    yum install ruby rubygems
 
到这里`rubygems`ruby的包管理软件安装完成.可以使用`gem`安装各种软件.

	     gem install gollum

# 可能遇到的问题

如果遇到以下问题

## 1 需要 gcc(编译) 
 
	yum install gcc

## 2 其他一些头文件

### 现象

    1. checking for libxml/parser.h... no
    2. checking for libxslt/xslt.h... no

### 解决方法

由于不同的包管理软件不能互通,`gem`需要的一些头文件可以使用`yum`安装   
`*.h`文件,一般包含在`dev`/`devel`包中. 在Fedora 18中使用`yum`安装指定的*-devel包即可.

    1. yum install libxml2-devel
    2. yum install libxslt-devel

总之一些`*.h`文件的错误可以先试试看安装对应的软件包的dev软件包.

## 3 rdoc编码

    ERROR:  While generating documentation for gollum-lib-1.0.3
    ... MESSAGE:   Unhandled special: Special: type=17, text="<!-- --- title: New Title -->"
    ... RDOC args: --ri --op /usr/lib/ruby/gems/1.8/doc/gollum-lib-1.0.3/ri --charset=UTF-8 --quiet lib README.md LICENSE --title gollum-lib-1.0.3         Documentation
    (continuing with the rest of the installation)
    Installing ri documentation for rack-1.5.2...
    Installing ri documentation for tilt-1.4.1...
    Installing ri documentation for rack-protection-1.5.0...
    Installing ri documentation for sinatra-1.4.3...

    unrecognized option `--encoding=UTF-8'

    For help on options, try 'rdoc --help'

试图手动安装:

    [root@test1 ~]# gem install rdoc
    Building native extensions.  This could take a while...
    Depending on your version of ruby, you may need to install ruby rdoc/ri data:

    <= 1.8.6 : unsupported
     = 1.8.7 : gem install rdoc-data; rdoc-data --install
     = 1.9.1 : gem install rdoc-data; rdoc-data --install
    >= 1.9.2 : nothing to do! Yay!
    Successfully installed json-1.8.0
    Successfully installed rdoc-4.0.1

发现由于的CentOS官方的rubygem版本过低,不支持.所以使用gems 1.8.7 on centos 6 参见 [这里](http://wiki.opscode.com/display/chef/Installing+Ruby+and+dependencies+on+CentOS+and+Others).之后升级`rubygems`:

    yum update rubygems

`gem --version`可以查看版本.

## 4 目录不支持UTF-8字符 

现象:在侧边栏(Sidebar)的目录(TOC)中不支持UTF-8字符,参见[[issues #547](https://github.com/gollum/gollum/issues/547)].

目前似乎没办法解决.依评论看可能与系统有关,本人本地计算机(Fedora 18)可以正常显示,但是VPS(CentOS)则显示乱码.

# 使用

## 运行服务

在wiki的目录下运行

	gollum

在浏览器中输入`127.0.0.1:4567`(默认)即可浏览/编辑.

如果出错很可能是wiki目录本身不是一个git仓库,最简单的方式是`git init`初始化一个仓库.

Tips:
* `--port` 选项可以指定端口 

## 编辑

* web页面新建,编辑,保存
* 直接编辑文件,`git commit`


## 保存/备份

* 本地文件
* git远程仓库([github](https://github.com/),[bitbucket](https://bitbucket.org/)等)
* [dropbox](https://www.dropbox.com/)等网盘

## 发布

* 服务器同样搭建gollum环境 (几乎与本地相同)
* apache ruby模块 (模式很复杂)
* [gollum-site](https://github.com/dreverri/gollum-site) 生成静态文件,发布. (有些bug未解决,不理想) [笔记](gollum-site)
* 发布到github某个项目的wiki页面 (省了服务器) 

### 内容更新

* 直接通过web编辑/保存
* web服务器从git仓库`git pull`抽取
* 手动/自动上传到webs,比如同步工具

# 参考

1. 发布到github的wiki页面: http://ju.outofmemory.cn/entry/28388 
2. gollum的git页面: https://github.com/gollum/gollum
