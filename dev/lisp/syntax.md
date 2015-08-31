# 简单语法


## 变量

`defvar`

## 函数

定义函数,使用`defun`

	(defun <函数名> <参数列表> <函数体>)
	(defun double (x) (* x 2))

	特别的例子
	(defun xxx nil 2)
	(defun x nil)


## lambda

	CL-USER> (lambda (x) (* x x))
	#<FUNCTION (LAMBDA (X)) {10052F8D6B}>

	CL-USER> ((lambda (x) (* x x)) 2)
	4


## 控制结构

### if-else


	(if <判断依据>
		<成立则这个值>
		<不成立这这个值>)

例子

	(if (> x 0)
		1
		0)
	(if (> x 0)
		1))

	(if (< x 0) (- x) x))
	(if 1 2 3)
	(if () 2 3)
	(if nil 2)

