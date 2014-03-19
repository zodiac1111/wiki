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
