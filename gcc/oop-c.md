# c实现部分的面向对象特征

# 具有默认参数的函数

类似c++中有默认值到函数参数.asni c89标准

注意点:
* 似乎只能用在参数列表的末尾. 宏`...`到特点
* 默认参数使用逗号运算必须避免不必要到副作用.
* `-Wall`显示逗号表达式有"没有效果到语句"即上面. 可用`#program`宏
* 似乎只能指定在参数列表中到一个参数拥有默认值


```c
#include <stdio.h>
#define SUM(a,...) sum( a, (5, ##__VA_ARGS__) )

int sum (a, b)
  int a;
  int b;
{
  return a + b;
}

int main()
{
  printf("%d\n", SUM( 3, 7 ) );
  printf("%d\n", SUM( 3 ) );
  return 0;
}
```

#得到变量类型 typeof

参考资料:http://module77.is-programmer.com/posts/22102.html

一个实用例子:
```c
#define SWAP(a,b) {  \
      typeof(a) _t=a;\
      a=b;           \
      b=_t;}
```