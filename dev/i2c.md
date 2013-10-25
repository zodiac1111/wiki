# i2c相关

## 设配规则

udev设配规则:https://xgoat.com/wp/2008/01/29/i2c-device-udev-rule/

```
KERNEL=="i2c-[0-9]*", GROUP="i2c"
```

## 一些参考 

I2C on a Linux based embedded system(pdf):  
http://home.deib.polimi.it/bellasi/lib/exe/fetch.php?media=students:giginghelli_finalreport.pdf

Behavioural analysis of an I2C Linux Driver(pdf) i2c linux 驱动行为分析:  
http://alexandria.tue.nl/repository/books/653250.pdf