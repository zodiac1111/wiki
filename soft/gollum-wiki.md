# gollum笔记
 
# 简介
[gollum](https://github.com/gollum/gollum)是一个wiki系统。github使用的就是gollum展示wiki页面。可以用于个人知识管理([PKM](http://en.wikipedia.org/wiki/Personal_knowledge_management))。本篇展现如何在Fedora系统上安装gollum。

特点:

* [git](http://git-scm.com/)操作/仓库
* markdown([wiki](http://en.wikipedia.org/wiki/Markdown))模块
* ruby([wiki_cn](http://zh.wikipedia.org/zh/Ruby)，[wiki](http://en.wikipedia.org/wiki/Ruby_(programming_language)))语言
* web编辑(自动commit)/本地编辑器编辑(手动commit后生效)

# 安装

需要 [`ruby`](http://en.wikipedia.org/wiki/Ruby_programming_language) [`rubygems`](http://en.wikipedia.org/wiki/RubyGems):

	# redhat
	yum install ruby rubygems ruby-devel libicu-devel
	# debian
	apt-get install ruby rubygems ruby-dev libicu-dev lib32z1-dev
 
到这里`rubygems`ruby的包管理软件安装完成.可以使用`gem`安装各种软件.

	sudo gem install gollum -V # -V 可以看到详细安装过程信息

是否要用sudo(root)用户安装gem? 不必须的情况下不要.[参考这里](http://stackoverflow.com/questions/2119064/sudo-gem-install-or-gem-install-and-gem-locations)

## 可能遇到的问题

如果遇到以下问题

### 需要 gcc(编译) 
 
	yum install gcc		# redhat
	apt-get install gcc	# debian

### 其他一些头文件

#### 现象

	1. checking for libxml/parser.h... no
	2. checking for libxslt/xslt.h... no

#### 解决方法

由于不同的包管理软件不能互通,`gem`需要的一些头文件可以使用`yum`安装   
`*.h`文件,一般包含在`dev`/`devel`包中. 在Fedora 18中使用`yum`安装指定的*-devel包即可.

	yum install libxml2-devel
	yum install libxslt-devel

总之一些`*.h`文件的错误可以先试试看安装对应的软件包的dev软件包.

例如:

	ERROR:  Error installing gollum:
	ERROR: Failed to build gem native extension.

	/usr/bin/ruby extconf.rb
	mkmf.rb can't find header files for ruby at /usr/lib/ruby/ruby.h

解决:

redhat系:

	yum install ruby-devel

debian系:

	apt-get install ruby-dev

#### 还是头文件

	[root@a ~]# gem install gollum
	Building native extensions.  This could take a while...
	ERROR:  Error installing gollum:
		ERROR: Failed to build gem native extension.

		    /usr/bin/ruby extconf.rb
	checking for main() in -licui18n... no
	which: no brew in (/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin)
	checking for main() in -licui18n... no


	***************************************************************************************
	*********** icu required (brew install icu4c or apt-get install libicu-dev) ***********
	***************************************************************************************

按照提示安装对应的包即可

	
	yum install libicu-devel	# redhat
	apt-get install libicu-dev	# debian

#### libz

	checking for unicode/ucnv.h... yes
	checking for main() in -lz... no
	libz missing
	*** extconf.rb failed ***
	Could not create Makefile due to some reason, probably lack of necessary
	libraries and/or headers.  Check the mkmf.log file for more details.  You may
	need configuration options.

	Provided configuration options:
		--with-opt-dir
		--without-opt-dir
		--with-opt-include
		--without-opt-include=${opt-dir}/include
		--with-opt-lib
		--without-opt-lib=${opt-dir}/lib
		--with-make-prog
		--without-make-prog
		--srcdir=.
		--curdir
		--ruby=/usr/bin/ruby2.1
		--with-icu-dir
		--without-icu-dir
		--with-icu-include
		--without-icu-include=${icu-dir}/include
		--with-icu-lib
		--without-icu-lib=${icu-dir}/lib
		--with-icui18nlib
		--without-icui18nlib
		--with-icui18nlib
		--without-icui18nlib
		--with-zlib
		--without-zlib
	ERROR:  Error installing gollum:
		ERROR: Failed to build gem native extension.

		Building has failed. See above output for more information on the failure.
	extconf failed, exit code 1

	Gem files will remain installed in /var/lib/gems/2.1.0/gems/charlock_holmes-0.7.3 for inspection.
	Results logged to /var/lib/gems/2.1.0/extensions/x86_64-linux/2.1.0/charlock_holmes-0.7.3/gem_make.out

参考 http://www.codeweavers.com/support/wiki/diag/missinglibz

Resolution

To install this library, run one of the following commands as root, or look for the corresponding package names in your favorite package manager:

    32-bit Debian or Ubuntu : apt-get install zlib1g
    64-bit Debian or Ubuntu : apt-get install lib32z1
    32/64-bit Fedora : yum install zlib.i686
    32/64-bit Mandriva : urpmi zlib1
    32-bit SUSE : zypper install libz1
    64-bit SUSE : zypper install libz1-32bit

需要dev包,包含头文件的.

比如debian系的 `lib32z1-dev`

### rdoc编码

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

发现由于的CentOS官方的rubygem版本过低,不支持
* 二进制: http://rpms.southbridge.ru/rhel6/ruby-1.9.3/x86_64/
* 所以使用gems 1.8.7 on centos 6,过程参见 这里:http://wiki.opscode.com/display/chef/Installing+Ruby+and+dependencies+on+CentOS+and+Others 
* 从源码编译 这里 :http://www.shiftedbytes.com/2011/04/install-ruby-192-passenger-on-centos-55.html (1.92)
* **目前方案** 从源码编译 这里: https://github.com/imeyer/ruby-1.9.2-rpm (ruby1.9.2) **gollum现在版本需要高版本的ruby**
* 或者这里 http://www.server-world.info/en/note?os=CentOS_6&p=ruby19

之后升级`rubygems`:

	yum update rubygems

`gem --version`可以查看版本.

### 目录不支持UTF-8字符 

现象:在侧边栏(Sidebar)的目录(TOC)中不支持UTF-8字符,参见[[issues #547](https://github.com/gollum/gollum/issues/547)].

目前似乎没办法解决.依评论看可能与系统有关,本人本地计算机(Fedora 18)可以正常显示,但是VPS(CentOS 6)则显示乱码.

Update:在`gollum 2.5.0`和`ruby 1.9.3`之下,这个问题已经没有再发生.项目更新非常快,请关注github页面.

### markdown文件后缀

由`gollum`默认生成的`markdown`文件默认的文件后缀是`.md`。而`.markdown`后缀也是可以被识别的。

但是，在web界面编辑`.markdown`后缀的文件后，git系统似乎只是将其"隐藏"了。在`git commit`之后表现出在web界面所有编辑都无效了。

暂时不知道其中原理,当前解决方案是将`markdown`文件后缀都统一改成`.md`。这样`gollum`编辑和git提交不再出现差异。 

# 使用

## 运行服务

在wiki的目录下运行

	gollum

在浏览器中输入`127.0.0.1:4567`(默认)即可浏览/编辑。

如果出错很可能是wiki目录本身不是一个git仓库,最简单的方式是`git init`初始化一个仓库。

 `--port` 选项可以指定端口 

## 编辑

* web页面新建、编辑、保存
* 直接编辑文件，`git commit`


## 保存/备份

* 本地文件
* git远程仓库([github](https://github.com/)，[bitbucket](https://bitbucket.org/)等)
* [dropbox](https://www.dropbox.com/)等网盘

## 发布

* 服务器同样搭建gollum环境 (几乎与本地相同)
	* `gollum --js --port 80`
* apache ruby模块 (貌似很复杂) 参考: [这里^](http://sailxjx.github.io/blog/blog/2012/07/12/ti-yan-gollum/)
* [gollum-site^](https://github.com/dreverri/gollum-site) 生成静态文件,发布。 (有些bug未解决,不理想) [笔记](gollum-site)
* 发布到github某个项目的wiki页面 (省了服务器) 

### 服务器部署

服务器可以安装gollum.定时从github上抓取

	crontab -e
	0 * * * * cd /root/wiki && git pull

有问题可以`mail`看看邮件.

启动启动(debian)

在 `/etc/rc.local` 下添加
```cd /root/wiki && /usr/local/bin/gollum --js --port=80```

### 内容更新

* 直接通过web编辑/保存
* web服务器从git仓库`git pull`抽取
* 手动/自动上传到webs，比如同步工具

# 参考

1. [发布到github的wiki页面](http://ju.outofmemory.cn/entry/28388 )
2. [gollum的git页面](https://github.com/gollum/gollum)

-----
# 更新

## 更新gollum本身

    $ sudo gem install gollum

## 更新时的错误

更新到gollum 2.5.1 (`sudo gem install gollum`)可能会遇到类似以下提示

	$ sudo gem install gollum
	Building native extensions.  This could take a while...
	ERROR:  Error installing gollum:
		ERROR: Failed to build gem native extension.

		    /usr/bin/ruby1.9.1 extconf.rb
	checking for main() in -licui18n... no
	checking for main() in -licui18n... no


	***************************************************************************************
	*********** icu required (brew install icu4c or apt-get install libicu-dev) ***********
	***************************************************************************************
	*** extconf.rb failed ***

按照提示`(brew install icu4c or apt-get install libicu-dev)`安装即可,在debian下即

	sudo apt-get install libicu-dev
