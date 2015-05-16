# lcd 相关

参考资料

http://blog.chinaunix.net/uid-27717694-id-3753746.html

液晶类型

* STN
* TFT


VSYNC（VFRAME）：帧同步信号
HSYNC（VLINE）：行同步信号
VCLK：像数时钟信号
VDEN（VM）：数据有效标志信号

```
//fb_fix_screeninfo：定义了显卡信息，如 framebuffer 内存的起始地址，地址长度等；
struct fb_fix_screeninfo {
    char id[16];    //字符串形式标示符
    unsigned long smem_start;//fb缓冲内存的开始地址（物理地址）
    __u32 smem_len;    //fb缓冲长度
    __u32 type; //FB_TYPE
    __u32 type_aux;    //Interleave for interleaved Planes
    __u32 visual;        //see FB_VISUAL_*    
    __u16 xpanstep;    //若没有硬件panning，赋0
    __u16 ypanstep;    //        /* zero if no hardware panning */
    __u16 ywrapstep;//        /* zero if no hardware ywrap */
    __u32 line_length;//1行的字节数
    unsigned long mmio_start;    //内存映射IO的开始地址（物理地址）
    __u32 mmio_len;    //内存映射IO的长度
    __u32 accel;//Indicate to driver which
    __u16 reserved[3];        /* Reserved for future compatibility */
};
```

```
//fb_var_screeninfo：描述了一种显卡显示模式的所有信息，如宽、高、颜色深度等，不同的显示模式对应不同的信息；
struct fb_var_screeninfo {
    __u32 xres;    //可见解析度
    __u32 yres;
    __u32 xres_virtual;//虚拟解析度
    __u32 yres_virtual;
    __u32 xoffset;//虚拟到可见之间的偏移
    __u32 yoffset; 

    __u32 bits_per_pixel;    //每像素位数BPP
    __u32 grayscale;//非0时指灰度
    
    //fb缓存的R、G、B位域
    struct fb_bitfield red;
    struct fb_bitfield green;
    struct fb_bitfield blue;
    struct fb_bitfield transp;//透明度

    __u32 nonstd;//非0时指非标准像素格式
    __u32 activate;//see FB_ACTIVATE_*
    __u32 height;    //高度
    __u32 width; //宽度
    __u32 accel_flags;//see fb_info.flags 

    //除了pixclock本身外，其余的都以像素时钟为单位
    __u32 pixclock;    //像素时钟（皮秒）
    __u32 left_margin;    //行切换，从同步到绘图之间的延迟
    __u32 right_margin;    //行切换，从绘图到同步之间的延迟
    __u32 upper_margin;    //帧切换，从同步到绘图之间的延迟
    __u32 lower_margin; //帧切换，从绘图到同步之间的延迟
    __u32 hsync_len; //水平同步的长度
    __u32 vsync_len; //垂直同步的长度
    __u32 sync;            //see FB_SYNC_*
    __u32 vmode;        //see FB_VMODE_*
    __u32 rotate;    //顺时钟旋转的角度
    __u32 reserved[5];    //保留
};
```
```
//fb_bitfield：描述每一像素显示缓冲区的组织方式，包括位域偏移、位域长度和MSB（最高有效位）指示
struct fb_bitfield {
    __u32 offset;//位域偏移
    __u32 length;//位域长度
    __u32 msb_right;//MSB（最高有效位）指示
};
```
