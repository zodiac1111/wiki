# 尾递归例子
```
(defun tailfact (n &optional (intermediate 1 ))
   (if (= n 1)
       (return-from tailfact intermediate))
   (tailfact (1- n) (* n intermediate)))
```