# hock the malloc function

* 水木讨论 http://www.newsmth.net/nForum/#!article/CProgramming/131503
* 引援2 http://www.linuxquestions.org/questions/programming-9/using-ld-wrap-function-to-instrument-790444/

# 例子

自己掌控 `malloc`

```c
#include <stdio.h>
#include <stdlib.h>

void *__real_malloc(size_t);

void *__wrap_malloc(size_t c)
{
        printf("My MALLOC called: %d\n", c);
        return __real_malloc(c);
}

int main (int argc, char *argv[])
{
        void *ptr = malloc(12);

        return 0;
}
```

编译

```bash
[gcc wrap]$ gcc wrap.c -o wrap -Wl,-wrap,malloc
[gcc wrap]$ ./wrap
My MALLOC called: 12
```

一个介绍 http://unixhelp.ed.ac.uk/CGI/man-cgi?ld
```text
--wrap symbol

	Use a wrapper function for symbol.  Any undefined reference to sym- 
	bol will be resolved to "__wrap_symbol".  Any  undefined  reference 
	to "__real_symbol" will be resolved to symbol. 

	This  can  be used to provide a wrapper for a system function.  The 
	wrapper function should be called "__wrap_symbol".  If it wishes to 
	call the system function, it should call "__real_symbol". 

	Here is a trivial example: 

		    void * 
		    __wrap_malloc (size_t c) 
		    { 
		      printf ("malloc called with %zu\n", c); 
		      return __real_malloc (c); 
		    } 

	If you link other code with this file using --wrap malloc, then all 
	calls to "malloc" will call the function  "__wrap_malloc"  instead. 
	The  call  to "__real_malloc" in "__wrap_malloc" will call the real 
	"malloc" function. 

	You may wish to provide a "__real_malloc" function as well, so that 
	links  without the --wrap option will succeed.  If you do this, you 
	should not put the definition of "__real_malloc" in the  same  file 
	as  "__wrap_malloc";  if you do, the assembler may resolve the call 
	before the linker has a chance to wrap it to "malloc". 
```
# 附

如果仅`malloc`那一套,gnu lib c 下可以 `man __malloc_hook`. "还得加锁，不好用"

`LD_PRELOAD`关键字可能也可以修改行为