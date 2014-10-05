# 指针指哪里
 
http://www.newsmth.net/nForum/#!article/CProgramming/131493?p=2


这个应该是 http://stackoverflow.com/a/11985451/1124787 . 
  
以下来自  The C Standard, 6.3.2.3, paragraph 7 [ISO/IEC 9899:2011] 和 [ISO/IEC 9899-1999] 
  
> A pointer to an object or incomplete type may be converted to a pointer to a different 
object or incomplete type. If the resulting pointer is not correctly aligned 57) for the 
pointed-to type, the behavior is undefined. Otherwise, when converted back again, the 
result shall compare equal to the original pointer. When a pointer to an object is 
converted to a pointer to a character type, the result points to the ** lowest ** addressed byte of 
the object. Successive increments of the result, up to the size of the object, yield pointers 
to the remaining bytes of the object. 
  
有人提到的怎样理解 http://stackoverflow.com/questions/24010608/how-to-interpret-section-6-3-2-3-part-7-of-the-c11-standard 
  
## OT 
* Upcasting Pointers 指针转来转去未定义. http://spin.atomicobject.com/2014/05/19/c-undefined-behaviors/ .可能引起下面的问题 
* 有的架构(编译器?) 不支持指针的 unaligned accesses http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.faqs/ka15414.html  
个人得出 像 
                0xffffff10  0xffffff11  0xffffff12  0xffffff13  
0xffffff10:       11          22          33           44  
如果以0xffffff13 这种不是4字节对齐来表示可能会有问题. 
当然因果关系不是这样的. 
* 各种坑 http://stackoverflow.com/a/13696253/1124787 
  