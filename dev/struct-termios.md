# termios结构体

>来源: http://www.mkssoftware.com/docs/man5/struct_termios.5.asp

data structure containing terminal information 

包含的终端信息的数据结构

# SYNOPSIS 概要
```c
#include <termios.h>
struct termios {
	tcflag_t c_iflag;
	tcflag_t c_oflag;
	tcflag_t c_cflag;
	tcflag_t c_lflag;
	cc_t c_cc[NCCS];
	speed_t c_ispeed;
	speed_t c_ospeed;
};
```
# DESCRIPTION 说明

The *termios* general terminal interface provides an interface to asynchronous communications devices. The NuTCRACKER Platform supports this interface for serial communication ports. This interface is also supported on the console window although the hardware-specific parts obviously do not apply.

*termios* 的通用终端接口提供了一个接口的异步通信设备

## Canonical Mode Input Processing  典型模式输入处理

In canonical mode input processing input is processed in units of lines. A line is delimited by a '\n' character or and end-of-file (EOF) character. A read request does not return until an entire line is read from the port or a signal is received. Also no matter how many bytes have been requested in the read call at most one line is returned. It is not necessary, however to read a whole line at once; any number of bytes even one may be requested without losing information.

If MAX_CANON is defined for the device it is a limit on the number of bytes in a line. The behavior of the system when this limit is exceeded is implementation-dependent. If MAX_CANON is not defined there is no such limit. Use the fpathconf() function to get the MAX_CANON setting for a device.

Erase and kill processing occurs when either of two special characters, the ERASE and KILL characters is received. This processing affects data in the canonical input queue that has not yet been delimited by a '\n' or EOF character. This un-delimited data makes up the current line. The ERASE character deletes the last character in the current line if any. The KILL character deletes all data in the current line. The ERASE and KILL characters have no effect if there is no data in the line and are themselves never placed in the input queue.

## Non-canonical Mode Input Processing 非典型模式输入处理