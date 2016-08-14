# 笔记

更成熟也更商业化的类似平台 http://crossbar.io/ *(考虑中)*

# 环境信息

hostname YZ 192.168.1.170 abao

# 代码

github `https://github.com/aktos-io/aktos-scada`

主要目录

	/home/abao/aktos-scada/test-folder/project/aktos-scada2/aktos-scada

# 前提

这两个都要

	aktos-dcs
	aktos-dcs-lib


# 编译

中间nodejs.可能版本有点旧

# 运行

## 前段缺少

`preparsed.js`

# 日常开发

## 修改

jade 源代码

/home/abao/aktos-scada/test-folder/project/aktos-scada2/aktos-scada/app/static

## 编译监视

 make development-compile-watch

## 相关信息

第三方组件,空间等位置 `aktos-scada/vendor`

`public` 是发布的符号连接文件


后台运行:

暂时先 screen

## 发布

生成发布更新, 开发服务器

	make production-update && make development-run-serve

更多 看 make


# 服务端推送

python manual-test/python-messaging/keypad_simulator.py  (github有的例子)

# 参考

## jade

[jade代码例子](http://jade-lang.com)

[Jade模板引擎使用](https://cnodejs.org/topic/5368adc5cf738dd6090060f2)

[Jade - 模板引擎](http://expressjs.jser.us/jade.html)

ls livescript


自动编译打开后显示编译如下:

	path is:  app/static/pages/scada/gms.jade
	path is:  app/static/pages/scada/izmirhs.jade
	path is:  app/static/pages/scada/mms.jade
	path is:  app/static/pages/scada/solaris.jade
	path is:  app/static/pages/scada/step-piston.jade
	path is:  app/static/pages/scada/thermometer-demo.jade
	path is:  app/static/pages/scada/trello-test.jade
	13 Aug 23:15:54 - info: compiled 115 files into 5 files, copied 1424 in 2725ms


