# sqlite相关

官网:(源代码) http://www.sqlite.org/download.html

公有领域.

Linux下编译 `gcc shell.c sqlite3.c -o sqlite3 -lpthread -ldl`

依赖线程库`-lpthread`:

```
/tmp/cc3Evh9t.o: In function `pthreadMutexAlloc':
sqlite3.c:(.text+0x3692): undefined reference to `pthread_mutexattr_init'
sqlite3.c:(.text+0x36a5): undefined reference to `pthread_mutexattr_settype'
sqlite3.c:(.text+0x36c2): undefined reference to `pthread_mutexattr_destroy'
/tmp/cc3Evh9t.o: In function `pthreadMutexTry':
sqlite3.c:(.text+0x3752): undefined reference to `pthread_mutex_trylock'
collect2: error: ld returned 1 exit status
```

依赖动态链接库`-ldl`:

```
/tmp/cc95EvvZ.o: In function `unixDlOpen':
sqlite3.c:(.text+0xcf9a): undefined reference to `dlopen'
/tmp/cc95EvvZ.o: In function `unixDlError':
sqlite3.c:(.text+0xcfac): undefined reference to `dlerror'
/tmp/cc95EvvZ.o: In function `unixDlSym':
sqlite3.c:(.text+0xcfea): undefined reference to `dlsym'
/tmp/cc95EvvZ.o: In function `unixDlClose':
sqlite3.c:(.text+0xd00f): undefined reference to `dlclose'
collect2: error: ld returned 1 exit status
```

使用小结: http://www.360doc.com/content/10/1020/20/3550092_62551572.shtml

打开/新建数据库:
```
sqlite3 test.db
```
查看帮助:
```
sqlite> .help
```
查看数据库:
```
sqlite> .databases
```
新建表:
```
sqlite> CREATE TABLE person (id INTEGER PRIMARY KEY AUTOINCREMENT, name VARCHAR(20), age SMALLINT);
```

查看表:
```
sqlite> .tables
```

详细表结构:
```
sqlite> .schema person 
```