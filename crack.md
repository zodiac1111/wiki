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

If we imagine code , it should look like this.

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