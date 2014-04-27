#bash shell编程

# 更通用的shebang

shebang: #!
```
#!/usr/bin/env
```

####特殊变量
	$0:脚本执行文件名
	$1 命令行参数1 ... 与main函数参数列表相似
	补全

###shell脚本调试
1. 脚本文件中添加`set -x`语句,显示每一步执行的语句.方便调试脚本

## 有用的技巧

我的所有bash脚本都以下面几句为开场白：

    #!/bin/bash
    set -o nounset
    set -o errexit

这样做会避免两种常见的问题：

* 引用未定义的变量(缺省值为“”)
* 执行失败的命令被忽略

参考 http://www.vaikan.com/bash-scripting/
