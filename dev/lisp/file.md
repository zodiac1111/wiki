#文件操作



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

# 参考

* [Lisp语言：文件操作](http://blog.csdn.net/keyboardota/article/details/8307317)
* [Lisp之文件系统](http://blog.chinaunix.net/uid-24774106-id-3483262.html)
* [14. Files and File I/O](http://www.gigamonkeys.com/book/files-and-file-io.html) (EN,详细)
* http://ezekiel.vancouver.wsu.edu/~cs355/lectures/lisp/io.pdf (EN)
