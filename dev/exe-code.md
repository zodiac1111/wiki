# 代码的段

http://www.theresistornetwork.com/2013/09/arm-bare-metal-programming.html

简单例子

```c
/*
 * Sample Program to Demonstrate Object Sections
 */

#include <stdint.h>

uint32_t i = 0x00;                  // .bss
uint32_t j;                         // .bss
uint32_t k = 100;                   // .data
char *phrase = "Andrew Rossignol";  // .rodata

int main(void) {                    // .text (all executable code)
    while(1);
    
    return 0;
}
```
An object file will have a number of sections, but we are specifically interested in:
* .bss
 * Global variables whose values are 0 (uninitialized or assigned)
* .data
 * Global variables whose values are initialized (non-zero)
* .rodata
 * Read only globally defined variables
* .text
 * Executable machine code