# 各种奇技淫巧

# c语言协程

一个“蝇量级” C 语言协程库

>  coroutine(co-operative routines) http://coolshell.cn/articles/10975.html

使用static 全局静态变量.线程不可重入.switch-case破裂

```
int function(void) {
  static int i, state = 0;
  switch (state) {
    case 0: goto LABEL0;
    case 1: goto LABEL1;
  }
  LABEL0: /* start of function */
  for (i = 0; i < 10; i++) {
    state = 1; /* so we will come back to LABEL1 */
    return i;
    LABEL1:; /* resume control straight after the return */
  }
}
```