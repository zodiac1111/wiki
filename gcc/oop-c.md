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

参考资料:
* http://module77.is-programmer.com/posts/22102.html
* http://gcc.gnu.org/onlinedocs/gcc/Typeof.html
* http://gcc.gnu.org/onlinedocs/gcc-3.3.5/gcc/Other-Builtins.html

iso c `__typeof__`


一个实用例子:
```c
#define SWAP(a,b) {  \
      typeof(a) _t=a;\
      a=b;           \
      b=_t;}
```

```c
#define max(a,b) \
  ({ typeof (a) _a = (a); \
     typeof (b) _b = (b); \
     _a > _b ? _a : _b; })
```

gnuc `__auto_type`
```c
#define max(a,b) \
       ({ __auto_type _a = (a); \
           __auto_type _b = (b); \
         _a > _b ? _a : _b; })
```

# 函数重载

参考:
* http://stackoverflow.com/questions/479207/function-overloading-in-c __builtin_types_compatible_p
* http://www.cnblogs.com/zenny-chen/p/3303560.html __builtin_choose_expr和_Generic


`__builtin_types_compatible_p` 在运行期根据参数列表中到参数类型进行重载.

配合`__builtin_choose_expr`可以实现编译器展开. c11标准支持`_Generic`等于两者之和.



```c
void printA(int a){
printf("Hello world from printA : %d\n",a);
}

void printB(const char *buff){
printf("Hello world from printB : %s\n",buff);
}

#define Max_ITEMS() 6, 5, 4, 3, 2, 1, 0 
#define __VA_ARG_N(_1, _2, _3, _4, _5, _6, N, ...) N
#define _Num_ARGS_(...) __VA_ARG_N(__VA_ARGS__) 
#define NUM_ARGS(...) (_Num_ARGS_(_0, ## __VA_ARGS__, Max_ITEMS()) - 1) 
#define CHECK_ARGS_MAX_LIMIT(t) if(NUM_ARGS(args)>t)
#define CHECK_ARGS_MIN_LIMIT(t) if(NUM_ARGS(args))
#define print(x , args ...) \
CHECK_ARGS_MIN_LIMIT(1) printf("error");fflush(stdout); \
CHECK_ARGS_MAX_LIMIT(4) printf("error");fflush(stdout); \
({ \
if (__builtin_types_compatible_p (typeof (x), int)) \
printA(x, ##args); \
else \
printB (x,##args); \
})

int main(int argc, char** argv) {
    int a=0;
    print(a);
    print("hello");
    return (EXIT_SUCCESS);
}
```

自己到简化(待验证).

* 似乎仅支持内建类型,
* 简化仅支持固定数量参数
* 千万小心宏展开

```c
#define print(x) 															\
	if (__builtin_types_compatible_p (__typeof__ (x), int)) {				\
		print_int(x); 														\
	}else if(__builtin_types_compatible_p (__typeof__ (x), char*)){	\
		print_char(x); 														\
	}else if(__builtin_types_compatible_p (__typeof__ (x), long)){	\
		print_long(x); 														\
	}else{																	\
		print_other(x); 													\
	}

void print_int(int a)
{
	printf("Hello world from print_int : %d\n",a);
}
void print_long(long a)
{
	printf("Hello world from print_long : %x\n",a);
}
void print_char(const char *buff)
{
	printf("Hello world from print_char : %s\n",buff);
}

int main(int argc, char** argv) {
	int a=0;
	long b=2;
	char *str="string";
	print(a);
	print(str);
	print(b);
	return 0;
}

```
例子3
```c
#define foo(x)                                                  \
({                                                           \
  typeof (x) tmp;                                             \
  if (__builtin_types_compatible_p (typeof (x), long double)) \
    tmp = foo_long_double (tmp);                              \
  else if (__builtin_types_compatible_p (typeof (x), double)) \
    tmp = foo_double (tmp);                                   \
  else if (__builtin_types_compatible_p (typeof (x), float))  \
    tmp = foo_float (tmp);                                    \
  else                                                        \
    abort ();                                                 \
  tmp;                                                        \
})
```