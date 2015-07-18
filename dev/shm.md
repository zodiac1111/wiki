# 

参考 http://serverfault.com/questions/173999/dump-a-linux-processs-memory-to-file


I'm not sure how you dump all the memory to a file without doing this repeatedly (if anyone knows an automated way to get gdb to do this please let me know), but the following works for any one batch of memory assuming you know the pid:

    $ cat /proc/[pid]/maps

This will be in the format (example):

```
00400000-00421000 r-xp 00000000 08:01 592398                             /usr/libexec/dovecot/pop3-login
00621000-00622000 rw-p 00021000 08:01 592398                             /usr/libexec/dovecot/pop3-login
00622000-0066a000 rw-p 00622000 00:00 0                                  [heap]
3e73200000-3e7321c000 r-xp 00000000 08:01 229378                         /lib64/ld-2.5.so
3e7341b000-3e7341c000 r--p 0001b000 08:01 229378                         /lib64/ld-2.5.so
```

Pick one batch of memory (so for example 00621000-00622000) then use gdb as root to attach to the process and dump that memory:

```
$ gdb --pid [pid]
(gdb) dump memory /root/output 0x00621000 0x00622000
```

Then analyse /root/output with the strings command, less you want the PuTTY all over your screen.