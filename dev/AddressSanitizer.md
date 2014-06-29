# AddressSanitizer

官方

* gcc的介绍 http://gcc.gnu.org/gcc-4.8/changes.html (更新日志),[详细](https://code.google.com/p/address-sanitizer/) 4.8以后支持
* clang的介绍 http://clang.llvm.org/docs/AddressSanitizer.html

个人

* [和qt一起的介绍](http://blog.qt.digia.com/blog/2013/04/17/using-gccs-4-8-0-address-sanitizer-with-qt/)

功能

* Out-of-bounds accesses to heap, stack and globals 越界
* Use-after-free 访问
* Use-after-return (to some extent)
* Double-free, invalid free 各种free的错误
* Memory leaks (experimental) 内存泄露(实现性的)

# 使用

简单的一个程序

```c
int main(void)
{
	int a[10];
	a[10]=2;
	return 0;
}
```

编译

    gcc test.c  -fsanitize=address

运行并查看结果
