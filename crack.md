# 面试时被要求破解一个程序

原文:http://erenyagdiran.github.io/I-was-just-asked-to-crack-a-program-Part-1/

首先试着运行一下

```bash
root@lisa:~# ./CrackTheDoor 

        *** DOOR CONTROL SYSTEM  ***



PASSWORD:
```

简单尝试几个密码,放弃.

看看文件

```bash
root@lisa:~# file CrackTheDoor 
CrackTheDoor: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.15, BuildID[sha1]=0x9927be2fe310bea01d412164103b9c8b2d7567ea, not stripped
root@lisa:~#
```

再看看链接的库

```bash
root@lisa:~# ldd CrackTheDoor 
    linux-gate.so.1 =>  (0xf777b000)
    libc.so.6 => /lib32/libc.so.6 (0xf760c000)
    /lib/ld-linux.so.2 (0xf777c000)
root@lisa:~#
```

* [linux-gate.so](http://www.trilithium.com/johan/2005/08/linux-gate/)

调试一下看看

```bash
root@lisa:~# gdb CrackTheDoor 
GNU gdb (GDB) 7.4.1-debian
Copyright (C) 2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>...
Reading symbols from /root/CrackTheDoor...(no debugging symbols found)...done.
(gdb) r
Starting program: /root/CrackTheDoor 

Program received signal SIGSEGV, Segmentation fault.
0x080484fb in __do_global_dtors_aux ()
(gdb)
```

看来有反调试

再调试,设置一个断点

```bash
root@lisa:~# gdb CrackTheDoor 
GNU gdb (GDB) 7.4.1-debian
Copyright (C) 2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>...
Reading symbols from /root/CrackTheDoor...(no debugging symbols found)...done.
(gdb) info file
Symbols from "/root/CrackTheDoor".
Local exec file:
    `/root/CrackTheDoor', file type elf32-i386.
    Entry point: 0x804762c
...
...
```

断点
```text
b * 0x804762c
```

按r运行
```text
gdb) x/30i $pc
=> 0x804762c: pusha  
   0x804762d:   mov    $0xaa,%dl
   0x804762f:   mov    $0x8048480,%edi
   0x8047634:   mov    $0x8048cbc,%ecx
   0x8047639:   mov    %edi,0x80476f3
   0x804763f:   mov    %ecx,0x80476f7
   0x8047645:   sub    %edi,%ecx
   0x8047647:   mov    $0x804762f,%esi
   0x804764c:   push   $0x80476c1
   0x8047651:   pusha  
   0x8047652:   mov    $0x55,%al
   0x8047654:   xor    $0x99,%al  ;< 这里
   0x8047656:   mov    $0x8047656,%edi
   0x804765b:   mov    $0x80476e5,%ecx
   0x8047660:   sub    $0x8047656,%ecx
   0x8047666:   repnz scas %es:(%edi),%al
   0x8047668:   je     0x804770a
   0x804766e:   mov    %edi,0x80476eb
   0x8047674:   popa   
   0x8047675:   add    0x80476eb,%edx
   0x804767b:   ret
```

0x8047654 in this address , we first put 0x55 to al register then xor it via 0x99 which produces 0xCC

0xCC is very important Because , It means "halt" in x86 architecture.When your debugger wants to stop your program , it swaps the bytes to 0xCC in where it wants to stop.


试试别的,追踪

```text
root@lisa:~# strace ./CrackTheDoor 
execve("./CrackTheDoor", ["./CrackTheDoor"], [/* 17 vars */]) = 0
[ Process PID=31085 runs in 32 bit mode. ]
brk(0)                                  = 0x9972000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xfffffffff7715000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY)      = 3
fstat64(3, {st_mode=S_IFREG|0644, st_size=35597, ...}) = 0
mmap2(NULL, 35597, PROT_READ, MAP_PRIVATE, 3, 0) = 0xfffffffff770c000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib32/libc.so.6", O_RDONLY)      = 3
read(3, "\177ELF\1\1\1\0\0\0\0\0\0\0\0\0\3\0\3\0\1\0\0\0\300o\1\0004\0\0\0"..., 512) = 512
fstat64(3, {st_mode=S_IFREG|0755, st_size=1441884, ...}) = 0
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xfffffffff770b000
mmap2(NULL, 1456504, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xfffffffff75a7000
mprotect(0xf7704000, 4096, PROT_NONE)   = 0
mmap2(0xf7705000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15d) = 0xfffffffff7705000
mmap2(0xf7708000, 10616, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xfffffffff7708000
close(3)                                = 0
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xfffffffff75a6000
set_thread_area(0xffe4d864)             = 0
mprotect(0xf7705000, 8192, PROT_READ)   = 0
mprotect(0x8049000, 4096, PROT_READ)    = 0
mprotect(0xf7733000, 4096, PROT_READ)   = 0
munmap(0xf770c000, 35597)               = 0
ptrace(PTRACE_TRACEME, 0, 0x1, 0)       = -1 EPERM (Operation not permitted)
ptrace(PTRACE_TRACEME, 0, 0x1, 0)       = -1 EPERM (Operation not permitted)

```


If you look at the last lines , the program crashed itself again.That's because ptrace syscall.

In linux , ptrace is an abbreviation for "Process Trace".With ptrace , you can control another process , changing its internal state like debuggers.

Debuggers use ptrace a lot :) it's their job. 调试器使用 ptrace

If we imagine code , it should look like this.程序类似这样

```c
int main()
{
    if (ptrace(PTRACE_TRACEME, 0, 1, 0) < 0) {
        printf("DEBUGGING... Bye\n");
        return 1;
    }
    printf("Hello\n");
    return 0;
}
```

By the way , you can only do once `ptrace[PTRACE_TRACEMe]` 只能ptrace一次  , so debugger ptraced the program before, there our call will return false so we figured out there is something out there controlling our program

捕获程序里的ptrace
```text
~/.gdbinit
set disassembly-flavor intel # Intel syntax is better
set disassemble-next-line on
catch syscall ptrace #Catch the syscall.
commands 1
set ($eax) = 0
continue
end
```

eax will hold the result of the syscall.And it's ia always 0 or let me say TRUE

this way , we bypass the ptrace protection and now we need to switch back to gdb

```
eren@lisa:~$ gdb ./CrackTheDoor 
GNU gdb (GDB) 7.4.1-debian
Copyright (C) 2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>...
Catchpoint 1 (syscall 'ptrace' [26])
Reading symbols from /home/eren/CrackTheDoor...(no debugging symbols found)...done.
(gdb) r
Starting program: /home/eren/CrackTheDoor 

Catchpoint 1 (call to syscall ptrace), 0x08047698 in ?? ()
=> 0x08047698:    3d 00 f0 ff ff   cmp    eax,0xfffff000

Catchpoint 1 (returned from syscall ptrace), 0x08047698 in ?? ()
=> 0x08047698:    3d 00 f0 ff ff   cmp    eax,0xfffff000

        *** DOOR CONTROL SYSTEM  ***



PASSWORD:
```

Ok , at least we can use our debugger as we want :)

i put another breakpoint here PJeGPC4TIVaKFmmy53DJ

```
Breakpoint 2, 0x08048534 in PJeGPC4TIVaKFmmy53DJ ()
=> 0x08048534 <PJeGPC4TIVaKFmmy53DJ+0>: 1e  push   ds
(gdb) x/40i $pc
=> 0x8048534 <PJeGPC4TIVaKFmmy53DJ>:    push   ds
   0x8048535 <PJeGPC4TIVaKFmmy53DJ+1>:    mov    ebp,esp
   0x8048537 <PJeGPC4TIVaKFmmy53DJ+3>:    sub    esp,0x20
   0x804853a <PJeGPC4TIVaKFmmy53DJ+6>:    mov    BYTE PTR [ebp-0x1],0xe4
   0x804853e <PJeGPC4TIVaKFmmy53DJ+10>:   mov    BYTE PTR [ebp-0x2],0x87
   0x8048542 <PJeGPC4TIVaKFmmy53DJ+14>:   mov    BYTE PTR [ebp-0x3],0xfb
   0x8048546 <PJeGPC4TIVaKFmmy53DJ+18>:   mov    BYTE PTR [ebp-0x4],0xbe
   0x804854a <PJeGPC4TIVaKFmmy53DJ+22>:   mov    BYTE PTR [ebp-0x5],0xc9
   0x804854e <PJeGPC4TIVaKFmmy53DJ+26>:   mov    BYTE PTR [ebp-0x6],0x93
   0x8048552 <PJeGPC4TIVaKFmmy53DJ+30>:   mov    BYTE PTR [ebp-0x7],0x84
   0x8048556 <PJeGPC4TIVaKFmmy53DJ+34>:   mov    BYTE PTR [ebp-0x8],0xfc
   0x804855a <PJeGPC4TIVaKFmmy53DJ+38>:   mov    BYTE PTR [ebp-0x9],0x8d
   0x804855e <PJeGPC4TIVaKFmmy53DJ+42>:   mov    BYTE PTR [ebp-0xa],0xe5
   0x8048562 <PJeGPC4TIVaKFmmy53DJ+46>:   mov    BYTE PTR [ebp-0xb],0xbf
   0x8048566 <PJeGPC4TIVaKFmmy53DJ+50>:   mov    BYTE PTR [ebp-0xc],0x5c
   0x804856a <PJeGPC4TIVaKFmmy53DJ+54>:   mov    BYTE PTR [ebp-0xd],0xe2
   0x804856e <PJeGPC4TIVaKFmmy53DJ+58>:   mov    BYTE PTR [ebp-0xe],0x76
   0x8048572 <PJeGPC4TIVaKFmmy53DJ+62>:   mov    BYTE PTR [ebp-0xf],0x21
   0x8048576 <PJeGPC4TIVaKFmmy53DJ+66>:   mov    BYTE PTR [ebp-0x10],0xb8
   0x804857a <PJeGPC4TIVaKFmmy53DJ+70>:   mov    DWORD PTR [ebp-0x14],0x0
   0x8048581 <PJeGPC4TIVaKFmmy53DJ+77>:   mov    eax,DWORD PTR [ebp-0x14]
   0x8048584 <PJeGPC4TIVaKFmmy53DJ+80>:   add    eax,DWORD PTR [ebp+0x8]
   0x8048587 <PJeGPC4TIVaKFmmy53DJ+83>:   movzx  eax,BYTE PTR [eax]
   0x804858a <PJeGPC4TIVaKFmmy53DJ+86>:   test   al,al
   0x804858c <PJeGPC4TIVaKFmmy53DJ+88>:   je     0x8048808 <PJeGPC4TIVaKFmmy53DJ+724>
   0x8048592 <PJeGPC4TIVaKFmmy53DJ+94>:   mov    eax,DWORD PTR [ebp-0x14]
   0x8048595 <PJeGPC4TIVaKFmmy53DJ+97>:   add    eax,DWORD PTR [ebp+0x8]
   0x8048598 <PJeGPC4TIVaKFmmy53DJ+100>:  mov    edx,DWORD PTR [ebp-0x14]
   0x804859b <PJeGPC4TIVaKFmmy53DJ+103>:  add    edx,DWORD PTR [ebp+0x8]
   0x804859e <PJeGPC4TIVaKFmmy53DJ+106>:  movzx  edx,BYTE PTR [edx]
   0x80485a1 <PJeGPC4TIVaKFmmy53DJ+109>:  xor    dl,BYTE PTR [ebp-0x1]
   0x80485a4 <PJeGPC4TIVaKFmmy53DJ+112>:  mov    BYTE PTR [eax],dl
   0x80485a6 <PJeGPC4TIVaKFmmy53DJ+114>:  add    DWORD PTR [ebp-0x14],0x1
   0x80485aa <PJeGPC4TIVaKFmmy53DJ+118>:  mov    eax,DWORD PTR [ebp-0x14]
   0x80485ad <PJeGPC4TIVaKFmmy53DJ+121>:  add    eax,DWORD PTR [ebp+0x8]
   0x80485b0 <PJeGPC4TIVaKFmmy53DJ+124>:  movzx  eax,BYTE PTR [eax]
   0x80485b3 <PJeGPC4TIVaKFmmy53DJ+127>:  test   al,al
   0x80485b5 <PJeGPC4TIVaKFmmy53DJ+129>:  je     0x804880b <PJeGPC4TIVaKFmmy53DJ+727>
   0x80485bb <PJeGPC4TIVaKFmmy53DJ+135>:  mov    eax,DWORD PTR [ebp-0x14]
   0x80485be <PJeGPC4TIVaKFmmy53DJ+138>:  add    eax,DWORD PTR [ebp+0x8]
   0x80485c1 <PJeGPC4TIVaKFmmy53DJ+141>:  mov    edx,DWORD PTR [ebp-0x14]
   0x80485c4 <PJeGPC4TIVaKFmmy53DJ+144>:  add    edx,DWORD PTR [ebp+0x8]
   0x80485c7 <PJeGPC4TIVaKFmmy53DJ+147>:  movzx  edx,BYTE PTR [edx]
   0x80485ca <PJeGPC4TIVaKFmmy53DJ+150>:  xor    dl,BYTE PTR [ebp-0x2]
```

Now this part is interesting

i see some constants moving somewhere and the inputs i gave to program xored with those constants

i continued to investigate more..

```
(gdb) x/30i X1bdrhN8Yk9NZ59Vb7P2
   0x8048838 <X1bdrhN8Yk9NZ59Vb7P2>:  sbb    ecx,DWORD PTR [ecx+0x20ec83e5]
   0x804883e <X1bdrhN8Yk9NZ59Vb7P2+6>:    mov    DWORD PTR [ebp-0x18],0x0
   0x8048845 <X1bdrhN8Yk9NZ59Vb7P2+13>:   mov    BYTE PTR [ebp-0x1],0xd9
   0x8048849 <X1bdrhN8Yk9NZ59Vb7P2+17>:   mov    BYTE PTR [ebp-0x2],0xcd
   0x804884d <X1bdrhN8Yk9NZ59Vb7P2+21>:   mov    BYTE PTR [ebp-0x3],0xc9
   0x8048851 <X1bdrhN8Yk9NZ59Vb7P2+25>:   mov    BYTE PTR [ebp-0x4],0xe5
   0x8048855 <X1bdrhN8Yk9NZ59Vb7P2+29>:   mov    BYTE PTR [ebp-0x5],0x9e
   0x8048859 <X1bdrhN8Yk9NZ59Vb7P2+33>:   mov    BYTE PTR [ebp-0x6],0xd0
   0x804885d <X1bdrhN8Yk9NZ59Vb7P2+37>:   mov    BYTE PTR [ebp-0x7],0xe8
   0x8048861 <X1bdrhN8Yk9NZ59Vb7P2+41>:   mov    BYTE PTR [ebp-0x8],0xa5
   0x8048865 <X1bdrhN8Yk9NZ59Vb7P2+45>:   mov    BYTE PTR [ebp-0x9],0xaf
   0x8048869 <X1bdrhN8Yk9NZ59Vb7P2+49>:   mov    BYTE PTR [ebp-0xa],0x87
   0x804886d <X1bdrhN8Yk9NZ59Vb7P2+53>:   mov    BYTE PTR [ebp-0xb],0xd2
   0x8048871 <X1bdrhN8Yk9NZ59Vb7P2+57>:   mov    BYTE PTR [ebp-0xc],0x79
   0x8048875 <X1bdrhN8Yk9NZ59Vb7P2+61>:   mov    BYTE PTR [ebp-0xd],0xa9
   0x8048879 <X1bdrhN8Yk9NZ59Vb7P2+65>:   mov    BYTE PTR [ebp-0xe],0x5d
   0x804887d <X1bdrhN8Yk9NZ59Vb7P2+69>:   mov    BYTE PTR [ebp-0xf],0x7
   0x8048881 <X1bdrhN8Yk9NZ59Vb7P2+73>:   mov    BYTE PTR [ebp-0x10],0x81
   0x8048885 <X1bdrhN8Yk9NZ59Vb7P2+77>:   mov    DWORD PTR [ebp-0x14],0x0
   0x804888c <X1bdrhN8Yk9NZ59Vb7P2+84>:   mov    eax,DWORD PTR [ebp-0x14]
   0x804888f <X1bdrhN8Yk9NZ59Vb7P2+87>:   add    eax,DWORD PTR [ebp+0x8]
   0x8048892 <X1bdrhN8Yk9NZ59Vb7P2+90>:   movzx  eax,BYTE PTR [eax]
   0x8048895 <X1bdrhN8Yk9NZ59Vb7P2+93>:   cmp    al,BYTE PTR [ebp-0x1]
   0x8048898 <X1bdrhN8Yk9NZ59Vb7P2+96>:   je     0x80488a2 <X1bdrhN8Yk9NZ59Vb7P2+106>
   0x804889a <X1bdrhN8Yk9NZ59Vb7P2+98>:   mov    eax,DWORD PTR [ebp-0x18]
```

This is also similar :) Now we pushing another bunch of constants....

Ok here's the remaining part of the function

```
0x804889d <X1bdrhN8Yk9NZ59Vb7P2+101>:    jmp    0x8048a20 <X1bdrhN8Yk9NZ59Vb7P2+488>
   0x80488a2 <X1bdrhN8Yk9NZ59Vb7P2+106>:  add    DWORD PTR [ebp-0x14],0x1
   0x80488a6 <X1bdrhN8Yk9NZ59Vb7P2+110>:  mov    eax,DWORD PTR [ebp-0x14]
   0x80488a9 <X1bdrhN8Yk9NZ59Vb7P2+113>:  add    eax,DWORD PTR [ebp+0x8]
   0x80488ac <X1bdrhN8Yk9NZ59Vb7P2+116>:  movzx  eax,BYTE PTR [eax]
   0x80488af <X1bdrhN8Yk9NZ59Vb7P2+119>:  cmp    al,BYTE PTR [ebp-0x2]
   0x80488b2 <X1bdrhN8Yk9NZ59Vb7P2+122>:  je     0x80488bc <X1bdrhN8Yk9NZ59Vb7P2+132>
   0x80488b4 <X1bdrhN8Yk9NZ59Vb7P2+124>:  mov    eax,DWORD PTR [ebp-0x18]
   0x80488b7 <X1bdrhN8Yk9NZ59Vb7P2+127>:  jmp    0x8048a20 <X1bdrhN8Yk9NZ59Vb7P2+488>
   0x80488bc <X1bdrhN8Yk9NZ59Vb7P2+132>:  add    DWORD PTR [ebp-0x14],0x1
   0x80488c0 <X1bdrhN8Yk9NZ59Vb7P2+136>:  mov    eax,DWORD PTR [ebp-0x14]
   0x80488c3 <X1bdrhN8Yk9NZ59Vb7P2+139>:  add    eax,DWORD PTR [ebp+0x8]
   0x80488c6 <X1bdrhN8Yk9NZ59Vb7P2+142>:  movzx  eax,BYTE PTR [eax]
   0x80488c9 <X1bdrhN8Yk9NZ59Vb7P2+145>:  cmp    al,BYTE PTR [ebp-0x3]
   0x80488cc <X1bdrhN8Yk9NZ59Vb7P2+148>:  je     0x80488d6 <X1bdrhN8Yk9NZ59Vb7P2+158>
   0x80488ce <X1bdrhN8Yk9NZ59Vb7P2+150>:  mov    eax,DWORD PTR [ebp-0x18]
   0x80488d1 <X1bdrhN8Yk9NZ59Vb7P2+153>:  jmp    0x8048a20 <X1bdrhN8Yk9NZ59Vb7P2+488>
   0x80488d6 <X1bdrhN8Yk9NZ59Vb7P2+158>:  add    DWORD PTR [ebp-0x14],0x1
   0x80488da <X1bdrhN8Yk9NZ59Vb7P2+162>:  mov    eax,DWORD PTR [ebp-0x14]
   0x80488dd <X1bdrhN8Yk9NZ59Vb7P2+165>:  add    eax,DWORD PTR [ebp+0x8]
---Type <return> to continue, or q <return> to quit---
   0x80488e0 <X1bdrhN8Yk9NZ59Vb7P2+168>:  movzx  eax,BYTE PTR [eax]
   0x80488e3 <X1bdrhN8Yk9NZ59Vb7P2+171>:  cmp    al,BYTE PTR [ebp-0x4]
   0x80488e6 <X1bdrhN8Yk9NZ59Vb7P2+174>:  je     0x80488f0 <X1bdrhN8Yk9NZ59Vb7P2+184>
   0x80488e8 <X1bdrhN8Yk9NZ59Vb7P2+176>:  mov    eax,DWORD PTR [ebp-0x18]
   0x80488eb <X1bdrhN8Yk9NZ59Vb7P2+179>:  jmp    0x8048a20 <X1bdrhN8Yk9NZ59Vb7P2+488>
   0x80488f0 <X1bdrhN8Yk9NZ59Vb7P2+184>:  add    DWORD PTR [ebp-0x14],0x1
   0x80488f4 <X1bdrhN8Yk9NZ59Vb7P2+188>:  mov    eax,DWORD PTR [ebp-0x14]
   0x80488f7 <X1bdrhN8Yk9NZ59Vb7P2+191>:  add    eax,DWORD PTR [ebp+0x8]
   0x80488fa <X1bdrhN8Yk9NZ59Vb7P2+194>:  movzx  eax,BYTE PTR [eax]
   0x80488fd <X1bdrhN8Yk9NZ59Vb7P2+197>:  cmp    al,BYTE PTR [ebp-0x5]
   0x8048900 <X1bdrhN8Yk9NZ59Vb7P2+200>:  je     0x804890a <X1bdrhN8Yk9NZ59Vb7P2+210>
   0x8048902 <X1bdrhN8Yk9NZ59Vb7P2+202>:  mov    eax,DWORD PTR [ebp-0x18]
   0x8048905 <X1bdrhN8Yk9NZ59Vb7P2+205>:  jmp    0x8048a20 <X1bdrhN8Yk9NZ59Vb7P2+488>
   0x804890a <X1bdrhN8Yk9NZ59Vb7P2+210>:  add    DWORD PTR [ebp-0x14],0x1
   0x804890e <X1bdrhN8Yk9NZ59Vb7P2+214>:  mov    eax,DWORD PTR [ebp-0x14]
```

Do you see the pattern that i see here ? If you not , no problem..

Here , the program compares the my xored inputs with the constants again.

Now , we look at the inputs again , first inputs were xored with some constants and outputs compared with other constants

So last 2 functions should be like this.

```c
void PJeGPC4TIVaKFmmy53DJ (int * p)
{
  int array[] = {0xe4,0x87,0xfb,0xbe,0xc9,0x93,0x84,0xfc,0x8d,0xe5,0xbf,0x5c,0xe2,0x76,0x21,0xb8}
  for(i=0;i<16;i++)
 {
    p[i] = p[i] ^ array[i]
 }
}

int X1bdrhN8Yk9NZ59Vb7P2(int * p)
{
   int array = {0xd9,0xcd,0xc9,0xe5,0x9e,0xd0,0xe8,0xa5,0xaf,0x87,0xd2,0x79,0xa9,0x5d,0x7,0x81}
   for(i=0;i<16;i++)
 {
    if(p[i] != array[i])
         return false; // fail..
 }
  return true 
}
```

So write up a simple python script to xor those two constants to find the key

```python
#!/usr/bin/python
firstConst = [0xe4,0x87,0xfb,0xbe,0xc9,0x93,0x84,0xfc,0x8d,0xe5,0xbf,0x5c,0xe2,0x76,0x21,0xb8]
secondConst = [0xd9,0xcd,0xc9,0xe5,0x9e,0xd0,0xe8,0xa5,0xaf,0x87,0xd2,0x79,0xa9,0x5d,0x7,0x81]
ret =""
for x in range(16):
        ret+=chr(firstConst[x] ^ secondConst[x])
print ret
```

```bash

eren@lisa:~$ ./CrackTheDoor 

        *** DOOR CONTROL SYSTEM  ***



PASSWORD: =J2[WClY"bm%K+&9

        ***  ACCESS GRANTED  ***

        ***  THE DOOR OPENED ***
```

后记:

看评论,使用 `strings` 可以作为第一个尝试的.

