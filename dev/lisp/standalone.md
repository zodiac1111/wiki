# 独立指令

使用buildapp编译lisp成为成为二进制(不可移植,效率不高,不依赖lisp环境)

# 示例文件

```cl
;;;; file: 1.lsip
;定义简单的helloworld函数
(defun main(argv)
  (format t "你好世界"))
;作为main入口,简单的打印一下
(main 1)
```

# 编译

```bash
buildapp --output b.out --load './1.lisp' --entry main
```
* `--output` 指明输出的二进制可执行程序的名字
* `--load` 加载lisp文件
* `--entry` 指明入口(?)

# 运行

```bash
$ ./b.out
你好世界
```

# 参考

* [buildapp](http://www.xach.com/lisp/buildapp/)
