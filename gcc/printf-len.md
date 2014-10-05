# 获得printf字符串长度

来源: [类似printf类函数，怎么获得格式化之后的字符串长度？ ](http://www.newsmth.net/nForum/#!article/CProgramming/131339)

`man va_start`

```
vsnprintf(NULL, 0, format, ...)
```

```c
int my_printf(const char *format, ...)  { 
     char * p_buffer; 
     va_list vl; 
     va_start(vl, format); 
     unsigned char len = vsnprintf((char *)NULL, 0, format, vl); 
     p_buffer = (char *)malloc(len); 
     if (p_buffer == NULL) 
         return -1; 
     vsprintf(p_buffer, format, vl); 
     va_end(vl); 
     // ... 
     将P_buffer中的内容输出至某设备（阻塞） 
     // ... 
     free(P_buffer); 
} 
```

> va_start 之后 vsnprintf，在这之后需要马上va_end吧？ 
否则对va的指针位置会有问题 
后面再重新va_start一次 

> 哦，忘了，va_list只能用一次 