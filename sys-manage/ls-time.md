# 定制`ls -l`输出的时间格式

## `--full-time`

参考man手册

## `--time-style`

设置`--time-style=<格式>`

可选值:
* `+<format>`:自定义格式
* `full-iso`
* `long-iso`
* `iso`
* `locale`
* `posix-style`

参考 http://www.gnu.org/software/coreutils/manual/html_node/Formatting-file-timestamps.html

关于第一个自定义格式如`--time-style="+%Y-%m-%d %H:%M:%S"`输出形如`2002-03-30 23:45:56`.

参考
* http://www.gnu.org/software/coreutils/manual/html_node/date-invocation.html#date-invocation
* http://www.gnu.org/software/coreutils/manual/html_node/Date-conversion-specifiers.html#Date-conversion-specifiers
* http://www.gnu.org/software/coreutils/manual/html_node/Time-conversion-specifiers.html#Time-conversion-specifiers