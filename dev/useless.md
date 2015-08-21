[5个stackoverflow上的欢乐主题](http://javarevisited.blogspot.com/2015/08/5-entertaining-posts-from-stackoverflow.html)

* [What is your best programmer joke?](http://stackoverflow.com/questions/234075/what-is-your-best-programmer-joke)
* [What is the best comment in source code you have ever encountered?](http://stackoverflow.com/questions/184618/what-is-the-best-comment-in-source-code-you-have-ever-encountered)
* [What's your favorite “programmer” cartoon?](http://stackoverflow.com/questions/84556/whats-your-favorite-programmer-cartoon)
* [New Programming Jargon](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)
* [Strangest language feature](http://stackoverflow.com/questions/1995113/strangest-language-feature)

# 杂交代码

[中文介绍](http://coolshell.cn/articles/2529.html),叫做[Polyglot](https://en.wikipedia.org/wiki/Polyglot_%28computing%29),[Stack Overflow 404 page](http://meta.stackoverflow.com/questions/252184/whats-the-joke-in-the-stack-overflow-404-page-code)也是实际的一个例子.

c和bash杂交[例子](http://coolshell.cn/articles/1824.html)

# 手动分配栈内存

`alloca`

[手动分配栈内存](http://blog.csdn.net/suoluotree/article/details/5649670)

已经不被推荐

# c语言协程

一个“蝇量级” C 语言协程库

>  coroutine(co-operative routines) http://coolshell.cn/articles/10975.html

使用static 全局静态变量.线程不可重入.switch-case破裂

```c
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

# 达夫设备

* [英文wiki](http://en.wikipedia.org/wiki/Duff's_device)
* [中文wiki](http://zh.wikipedia.org/wiki/%E8%BE%BE%E5%A4%AB%E8%AE%BE%E5%A4%87)
* [how-does-duffs-device-work](http://stackoverflow.com/questions/514118/how-does-duffs-device-work)

# 参数名传递参数

按名称传递参数,可以指定任一顺序的参数,只需要之处参数名称,可以使用默认值,巧用结构体.

lisp也叫关键字参数 

* [Keyword arguments in C](http://www.darkcoding.net/software/keyword-arguments-in-c/) 
* 有限价值的参考:http://c2.com/cgi/wiki?SimulatingKeywordArguments

头文件
```c
#define uart_led_blink(...) led_blink_fun((struct led_blink_args){__VA_ARGS__})
/// 一个串口拥有的led,两个:收/发
enum eUartLed {
	eUartLed_TX = 0,
	eUartLed_RX,
};
/// 串口led闪烁函数的参数列表
struct led_blink_args {
	DrvGPIOCallBackFun blink;
	int comNo;
	enum eUartLed led;
	int count;
	int delay;
};
```
实现
```c
/**
 * led_blink_fun闪烁函数
 * @param args	led_blink_args 参数列表,包含下面的一系列参数
 * @param blink	DrvGPIOCallBackFun gpio闪烁函数
 * @param comNo 串口序号 1,2,3,4 从1开始
 * @param led[enum eUartLed]	哪个led,每个串口通道包含发送和接收两个led
 * @param count	[可选]闪烁的次数(默认1次)
 * @param delay	[可选]闪烁的间隔时间,亮delay毫秒,灭delay毫秒(默认100ms)
 * 调用方式(配合uart_led_blink宏),如:
 * * uart_led_blink( p,no, eUartLed_TX,1,100)
 * * uart_led_blink( p,no, eUartLed_TX)
 * * uart_led_blink( p,no, eUartLed_TX,1)
 * * uart_led_blink( p,no, eUartLed_TX,.count=2,.delay=50)
 * * uart_led_blink( p,no, eUartLed_TX,.delay=1000)
 * @retval 0 成功
 */
int led_blink_fun(struct led_blink_args args)
{
	int i;
	int iled = 0;
	/// 默认闪烁一次
	int count = args.count ? : 1;
	/// 闪烁间隔时间,毫秒,亮100毫秒,灭100毫秒
	int delay = args.delay ? : 100;
	/// 实际的gpio端口
	int gpio_port = 0;
	if (args.comNo<1||args.comNo>MAX_COM_NUM) {
		CLOG_ERR("串口序号不正确应该取[1,%d],放弃闪烁", args.comNo);
		return -1;
	}
	CLOG_INFO("blink =%p comNo =%d count=%d delay=%d"
		, args.blink
		, args.comNo
		, count, delay);
	switch (args.led) {
	case eUartLed_TX:
		iled = 0;
		break;
	case eUartLed_RX:
		iled = 4;     ///同一端口收灯和发灯之间的数值间隔
		break;
	default:
		CLOG_ERR("未知的uart ledl类型 %d", args.led);
		return -2;
		break;
	}
	gpio_port = args.comNo-1;     ///先从base1的串口号转化成为baseon0的序号
	gpio_port += WEBBOX_2UART_VER_COM_NUMBER_OFFSET;     /// 2串口类型的偏移量
	gpio_port += iled;     /// 收 or 发的led的偏移量
	args.blink(gpio_port, GPIO_LED_OFF);
	for (i = 0; i<count; i++) {
		args.blink(gpio_port, GPIO_LED_ON);
		usleep(delay*1000);
		args.blink(gpio_port, GPIO_LED_OFF);
		usleep(delay*1000);
	}
	return 0;
}
```