# avr-gcc 编译过程

参考eclispe-avr插件对avr-gcc程序的编译过程

编译
```
  avr-gcc -Wall -g3 -gstabs -Os -fpack-struct -fshort-enums -std=gnu99 -funsigned-char -funsigned-bitfields -mmcu=atmega8 -DF_CPU=8000000UL -MMD -MP -MF"LCDTestC.d" -MT"LCDTestC.d" -c -o "LCDTestC.o" "../LCDTestC.c"
```
链接 
```
  avr-gcc -Wl,-Map,lcd_8bit.map -mmcu=atmega8 -o "lcd_8bit.elf"  ./HD44780.o ./LCDTestC.o ./aux_globals.o   
```
获取lss(可选)
```
  avr-objdump -h -S lcd_8bit.elf  >"lcd_8bit.lss"
```
Create Flash image (ihex format)
```
  avr-objcopy -R .eeprom -O ihex lcd_8bit.elf  "lcd_8bit.hex"
```
Create eeprom image (ihex format)(按照需求)
```
  avr-objcopy -j .eeprom --no-change-warnings --change-section-lma .eeprom=0 -O ihex lcd_8bit.elf  "lcd_8bit.eep"
```
下载
```
  /usr/bin/avrdude -pm8 -cusbasp -B500 -i500 -Uflash:w:lcd_8bit.hex:a
```