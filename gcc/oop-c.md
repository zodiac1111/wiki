# c实现部分的面向对象特征

# 具有默认参数的函数
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