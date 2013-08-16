# gcc 实用 attribute

## 限定函数作用范围

```
int foo(int x,int y) __attribute__((const));
```

foo中不会修改全局的变量.类似c++ const成员函数不会修改成员变量.比较强

```
int foo(int x,int y) __attribute__((pure));
```
与上面相似,函数返回值之与参数有关,不会修改全局变量.每次调用返回值都一样.让编译器知道优化选项.多次调用则仅计算一次,之后会记下返回值.