# shell in a box

**未完成**

移植到arm.

官网:http://code.google.com/p/shellinabox/

# 期间错误

## 版本不对

```text
libtool: Version mismatch error.  This is libtool 2.2.6b Debian-2.2.6b-2ubuntu1, but the
libtool: definition of this LT_INIT comes from libtool 2.2.10.
libtool: You should recreate aclocal.m4 with macros from libtool 2.2.6b Debian-2.2.6b-2ubuntu1
libtool: and run autoconf again.
```

参考 http://askubuntu.com/questions/56937/babl-recreate-aclocal-m4-with-macros-from-libtool-2-4 和 http://stackoverflow.com/questions/3096989/libtool-version-mismatch-error 执行:
```bash
autoreconf --force --install
./configure
make
```

其中configure:
```bash
./configure CC=arm-linux-gcc --host=arm --prefix='$HOME/Downloads/shellinabox-2.14/install'
```