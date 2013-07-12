# gollum wiki 使用
 
## 简介
[gollum](https://github.com/gollum/gollum)是一个wiki系统.github使用的就是gollum.可以用于个人知识管理.

* git操作,仓库
* markdown模块
* ruby语言
* web编辑(自动commit)/本地编辑器编辑(手动commit后生效)

## 安装


需要 `ruby`([wiki](http://en.wikipedia.org/wiki/Ruby_programming_language)) `rubygems`([wiki](http://en.wikipedia.org/wiki/RubyGems))

 `yum install ruby rubygems`
 
到这里`rubygems`ruby的包管理软件安装完成.可以使用`gem`安装各种软件.
 
 `gem install gollum`

## 可能遇到的问题

如果遇到以下问题

### 1 需要 gcc(编译) 
 
 `yum install gcc`

### 2 其他一些头文件

#### 现象

    1. checking for libxml/parser.h... no
    2. checking for libxslt/xslt.h... no

#### 对应解决:  
由于不同的包管理软件不能互通,`gem`需要的一些头文件可以使用`yum`安装   
`*.h`文件,一般包含在`dev`/`devel`包中. 在Fedora 18中使用`yum`安装指定的*-devel包即可.

    1. yum install libxml2-devel
    2. yum install libxslt-devel

### 3编码

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

由于我的centos官方的rubygem版本过低,不支持.所以使用gems 1.8.7 on centos 6 参见 [这里](http://wiki.opscode.com/display/chef/Installing+Ruby+and+dependencies+on+CentOS+and+Others).之后升级`rubygems`:

    yum update rubygems

`gem --version`可以查看版本.

## 使用

待定

zzzzz

xxxxx

ccccc
