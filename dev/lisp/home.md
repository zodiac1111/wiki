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

# 语法

[lisp语法](lisp-syntax)
[脱离环境独立运行](lisp-standalone)



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