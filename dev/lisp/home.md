# lisp相关

# 类别

[ide emacs+slime+(sbcl)](https://nabacg.wordpress.com/2012/02/26/emacs-sbcl-slime-common-lisp-environment-on-windows/)

# 启动

    M-x slime

# helloworld

    "hello,world!"

# load pragram

    (load "~/workspace/lisp1/hello.lisp")

# 一些快捷键

* `M-p` 重复上一次输入类似shell上箭头(Previous),相反是`M-n`(Next)

变量 defvar

函数 defun

```
(defun double (x) (* x 2))
(defun 函数名 参数列表 函数体)

特别的例子
(defun xxx nil 2)
(defun x nil)
```

lambda

```text
CL-USER> (lambda (x) (* x x))
#<FUNCTION (LAMBDA (X)) {10052F8D6B}> 

CL-USER> ((lambda (x) (* x x)) 2)
4    
```

控制结构

if
```text
(if (< x 0) (- x) x))
(if 判断依据
    成立则这个值
    不成立这这个值)
例子
(if (> x 0) 
    1
    0)
(if (> x 0) 
    1))

(if 1 2 3)
(if () 2 3)
(if nil 2)
```
## sbcl

# 独立的可执行程式

## buildapp
使用buildapp编译lisp成为成为二进制(不可移植,效率不高,不依赖lisp环境)

参考: http://www.xach.com/lisp/buildapp/

```cl
;;;; file: 1.lsip
;定义简单的helloworld函数
(defun main(argv)           
  (format t "你好世界"))
;作为main入口,简单的打印一下
(main 1)
```

编译

```bash
buildapp --output b.out --load './1.lisp' --entry main
```
* `--output` 指明输出的二进制可执行程序的名字
* `--load` 加载lisp文件
* `--entry` 指明入口(?)

运行
```
$ ./b.out 
你好世界
```

#文件操作

* http://blog.csdn.net/keyboardota/article/details/8307317
* http://blog.chinaunix.net/uid-24774106-id-3483262.html
* http://www.gigamonkeys.com/book/files-and-file-io.html (EN,详细)
* http://ezekiel.vancouver.wsu.edu/~cs355/lectures/lisp/io.pdf (EN)

##打开
```cl
;;;;最简单,其返回"文件描述符"
(open "/path/to/file.txt")
;;;; 打开不存在的文件,返回nil
(open "/path/to/file.txt" :if-does-not-exist NIL)
```
## 读文件
```cl
;;;; 用read-line 显示一下
(format t "~a~%" (read-line (open "1.c")))
```
## 写文件
```cl
;;;;以写方式打开/新建
(open "new_file" :direction :output :if-exists :supersede)
;;;;
(defun write-file (filename content)
    (let ((stream (open filename :direction :output:if-exists :supersede)))
        (format stream "~A ~%" content)
        (close stream)))
(write-file "file_w" "hello world")
```