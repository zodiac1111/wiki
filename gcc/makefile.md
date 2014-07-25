# makefile

## 约定的变量

* 百度空间 http://hi.baidu.com/qruzstqoyhbeisd/item/13837831a59527617c034bcd
 * CC 与 CXX 编译器
 * CPPFLAGS  预处理阶段的选项
 * CFLAGS 与 CXXFLAGS 编译器的选项
 * LDFLAGS  连接器的选项

* Linux C编程一站式学习 http://learn.akae.cn/media/ch22s03.html
* gnu组织 http://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html

## 条件语句 逻辑运算

条件语句

make没有`if-elseif-endif`语句。只能使用`if-else`嵌套

```bash
ifeq ($(ARCH),arm)
  CC=arm-linux-gcc
endif

ifeq ($(CC),gcc)
  libs=$(libs_for_gcc)
else
  libs=$(normal_libs)
endif
```

## shell输出作为变量

参考 http://stackoverflow.com/questions/2373081/assign-a-makefile-variable-value-to-a-bash-command-result

注意可能需要的转义字符`$`等

```makefile
BUILD_NUM:=$(shell git rev-list --all|wc -l)
BUILD_TIME:=$(shell date +%s)
```