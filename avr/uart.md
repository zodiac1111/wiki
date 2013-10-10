#通用异步收发传输器

> http://zh.wikipedia.org/zh/UART

通用异步收发传输器（Universal Asynchronous Receiver/Transmitter，通常称作UART，读音/ˈjuːart/）

###TTL

原始是TTL电平

包括了[RS232](http://zh.wikipedia.org/wiki/RS-232)、RS449、RS423、[RS422](http://zh.wikipedia.org/wiki/EIA-422)和[RS485](http://zh.wikipedia.org/wiki/RS-485)等接口标准规范和总线标准规范(电平定义)

###485 
RS485英文:http://en.wikipedia.org/wiki/RS-485

典型的网络连接:

[[http://upload.wikimedia.org/wikipedia/commons/thumb/9/96/Rs485-bias-termination.svg/220px-Rs485-bias-termination.svg.png]]

###485波形传输例子 Waveform example

The diagram below shows potentials of the '+' and '−' pins of an RS-485 line during transmission of one byte (0xD3, least significant bit first) of data using an asynchronous start-stop method.

[[http://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/RS-485_waveform.svg/487px-RS-485_waveform.svg.png]]


RS485和RS232电器引脚比较:http://digital.ni.com/public.nsf/3efedde4322fef19862567740067f3cc/2ef59b02fffccb3486256914006442ef?OpenDocument

由于历史原因，IBM的PC外部接口配置为RS232，成为实际上的PC界默认标准。所以，现在PC机的COM均为RS232。若配有多个异步串行通信口，则分别称为COM1、COM2... 。


Many UARTs have a small first-in, first-out FIFO buffer memory between the receiver shift register and the host system interface.(**avr的移位寄存器?**)

###Structure结构

A UART usually contains the following components:
* a clock generator, usually a multiple of the bit rate to allow sampling in the middle of a bit period. 时钟
* input and output shift registers 寄存器
* transmit/receive control  控制接受发送,如[RS485](http://zh.wikipedia.org/wiki/RS-485)这样的[半双工](http://zh.wikipedia.org/wiki/%E5%8D%8A%E9%9B%99%E5%B7%A5)需要控制
* read/write control logic
* transmit/receive buffers (optional) 可选,缓冲区 **注意**
* parallel data bus buffer (optional)
* First-in, first-out (FIFO) buffer memory (optional)

##参考资料
* <http://en.wikipedia.org/wiki/Universal_asynchronous_receiver/transmitter>
* <http://zh.wikipedia.org/zh/UART>
* 波特率 baud http://en.wikipedia.org/wiki/Baud 和 http://zh.wikipedia.org/wiki/%E6%B3%A2%E7%89%B9%E7%8E%87
* 比特率 http://en.wikipedia.org/wiki/Bit_rate 
* RS232教程 http://www.camiresearch.com/Data_Com_Basics/RS232_standard.html