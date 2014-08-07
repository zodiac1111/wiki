# octopress 博客基本操作

octopress blog基本操作  
http://octopress.org/docs/blogging/

# 安装

gem:
`yum install gem`

bundler:
`gem install bundler`

rake:
`sudo gem install rake`

Could not find rake-0.9.6 in any of the sources
`bundle install`

git clone 项目.
rake等操作

# 新建文章(post)

```bash
#传到博客根目录
cd octopress
#新建日志
rake new_post["Title"]
```

编辑生成文章

```text
---
layout: post
title: "可以改成中文名"
date: 2013-10-28 11:32
comments: true    #评论
categories:       #分类
---
```

然后下面就可以开始以markdowm格式撰写文章了.

# 预览&发布 

```bash
#在本地预览blog
#转到目录
cd /home/zodiac1111/blog/octopress
#生成
rake generate
#预览 在本地输入 http://localhost:4000/ 访问
rake preview
#发布
rake deploy	
#生成和发布也可以合并成为一句:
rake gen_deploy
```

## 保存source(可选)
```bash
#保存博客源代码之 页面的source分支下。
cd /home/zodiac1111/blog/octopress
git add .
git commit -m 'blog source update'
#推送至远端
git push origin source
```

# 更新octopress博客程序
```bash
cd octopress
git pull git://github.com/imathis/octopress.git #pull官方程序
```

# 顺带:rake目标补全

参考 https://github.com/ai/rake-completion

Ubuntu

Add Ubuntu on Rails PPA:

    $ sudo add-apt-repository ppa:ubuntu-on-rails/ppa

Install `rake-completion` package:

    $ sudo apt-get install rake-completion

其他类UNIX系统

全局

复制`rake`脚本到`/etc/bash_completion.d/`.

用户

Copy rake script (for example, to ~/scripts/) and add to your .bashrc:

    . ~/scripts/rake