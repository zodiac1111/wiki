# 尾递归例子

## 范例
递归版本(数学逻辑)

```
(defun fact (n)
   (if (= n 1 )
       1
       (* n (fact (1- n)))))
```

尾递归版本(计算机科学优化)

```
(defun tailfact (n &optional (intermediate 1 ))
   (if (= n 1)
       (return-from tailfact intermediate))
   (tailfact (1- n) (* n intermediate)))
```

## 运行


```
(fact 10000)
(tailfact 10000)
```
注: lispbox上的cl也许进行了尾递归优化,没发现差别,还编译(compile)了,运行效率高.
