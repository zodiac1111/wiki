# crt

[crt](http://en.wikipedia.org/wiki/Crt0)


这样才可以 
```
ld -dynamic-linker /lib/ld-linux.so.2 /usr/lib/crt1.o /usr/lib/crti.o -lc hello.o /usr/lib/crtn.o 
```
hello.c源码 
```
int main() 
{ 
printf("hello"); 
} 
```
生成hello.o 
```
gcc -c hello.c 
```

然后用 
ld -dynamic-linker /lib/ld-linux.so.2 /usr/lib/crt1.o /usr/lib/crti.o -lc hello.o /usr/lib/crtn.o 

生成a.out 