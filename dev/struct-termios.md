# termios结构体

>来源: http://www.mkssoftware.com/docs/man5/struct_termios.5.asp

data structure containing terminal information 

包含的终端信息的数据结构

#SYNOPSIS 概要
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