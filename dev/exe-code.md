# 代码的段

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