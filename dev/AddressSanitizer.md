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

	zodiac1111@debian:tmp$ ./a.out 
	=================================================================
	==13610== ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7fff89250f48 at pc 0x400742 bp 0x7fff89250ef0 sp 0x7fff89250ee8
	WRITE of size 4 at 0x7fff89250f48 thread T0
		#0 0x400741 (/home/zodiac1111/tmp/a.out+0x400741)
		#1 0x7faee8230b44 (/lib/x86_64-linux-gnu/libc-2.19.so+0x21b44)
		#2 0x400608 (/home/zodiac1111/tmp/a.out+0x400608)
	Address 0x7fff89250f48 is located at offset 72 in frame <main> of T0's stack:
	  This frame has 1 object(s):
		[32, 72) 'a'
	HINT: this may be a false positive if your program uses some custom stack unwind mechanism or swapcontext
		  (longjmp and C++ exceptions *are* supported)
	Shadow bytes around the buggy address:
	  0x100071242190: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000712421a0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000712421b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000712421c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000712421d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	=>0x1000712421e0: f1 f1 f1 f1 00 00 00 00 00[f4]f4 f4 f3 f3 f3 f3
	  0x1000712421f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100071242200: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100071242210: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100071242220: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100071242230: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	Shadow byte legend (one shadow byte represents 8 application bytes):
	  Addressable:           00
	  Partially addressable: 01 02 03 04 05 06 07 
	  Heap left redzone:     fa
	  Heap righ redzone:     fb
	  Freed Heap region:     fd
	  Stack left redzone:    f1
	  Stack mid redzone:     f2
	  Stack right redzone:   f3
	  Stack partial redzone: f4
	  Stack after return:    f5
	  Stack use after scope: f8
	  Global redzone:        f9
	  Global init order:     f6
	  Poisoned by user:      f7
	  ASan internal:         fe
	==13610== ABORTING

