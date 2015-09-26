

expect

spawn: command not found

```
#!/usr/bin/expect -f
spawn telnet 192.168.1.1
expect Login:
send "root\n"
expect Password:
send "LY9x25\n"
```