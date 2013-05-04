#通用异步收发传输器

> http://zh.wikipedia.org/zh/UART

通用异步收发传输器（Universal Asynchronous Receiver/Transmitter，通常称作UART，读音/ˈjuːart/）

原始是TTL电平

包括了RS232、RS449、RS423、RS422和RS485等接口标准规范和总线标准规范(电平定义)

由于历史原因，IBM的PC外部接口配置为RS232，成为实际上的PC界默认标准。所以，现在PC机的COM均为RS232。若配有多个异步串行通信口，则分别称为COM1、COM2... 。

Many UARTs have a small first-in, first-out FIFO buffer memory between the receiver shift register and the host system interface.(**avr的移位寄存器?**)

###Structure结构

A UART usually contains the following components:
* a clock generator, usually a multiple of the bit rate to allow sampling in the middle of a bit period. 时钟
* input and output shift registers 寄存器
* transmit/receive control  控制接受发送,如485单双工需要控制
* read/write control logic
* transmit/receive buffers (optional) 可选,缓冲区**注意**
* parallel data bus buffer (optional)
* First-in, first-out (FIFO) buffer memory (optional)

##参考资料
* <http://en.wikipedia.org/wiki/Universal_asynchronous_receiver/transmitter>
* <http://zh.wikipedia.org/zh/UART>
* 波特率 baud http://en.wikipedia.org/wiki/Baud http://zh.wikipedia.org/wiki/%E6%B3%A2%E7%89%B9%E7%8E%87
* 比特率 http://en.wikipedia.org/wiki/Bit_rate 