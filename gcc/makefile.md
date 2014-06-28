# makefile

# 约定的变量

* 百度空间 http://hi.baidu.com/qruzstqoyhbeisd/item/13837831a59527617c034bcd
 * CC 与 CXX 编译器
 * CPPFLAGS  预处理阶段的选项
 * CFLAGS 与 CXXFLAGS 编译器的选项
 * LDFLAGS  连接器的选项

* Linux C编程一站式学习 http://learn.akae.cn/media/ch22s03.html
* gnu组织 http://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html

# 条件语句 逻辑运算

条件语句
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
