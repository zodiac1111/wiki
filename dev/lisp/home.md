# lisp相关

# 类别

## sbcl

## buildapp

使用buildapp编译lisp成为成为二进制(不可移植,效率不高,不依赖lisp环境)

```
;;;; file: 1.lsip
;定义简单的helloworld函数
(defun main(argv)           
  (format t "你好世界")
;作为main入口,简单的打印一下
(main 1)
```
