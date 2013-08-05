# avr 入门教程

**来源**:http://www.micahcarrick.com/tutorials/avr-microcontroller-tutorial/introduction-to-avr-microcontrollers.html

仅取linux部分.

# Introduction 简介

## What Are AVR Microcontrollers? AVR微处理器是什么
## What You Should Already Know 预备知识
## Tools and Equipment (Toys) 工具和装备
## Software and Development Environment 软件和开发环境 
## Overview of Development Process 开发过程概览

# Getting Started: Blinking an LED 入门:闪烁的LED
## List of Materials 材料单

* One of the following AVR Microcontrollers: ATmega328, ATmega168, ATmega88, or ATmega48. An older ATmega8 or similar may also work but has not been tested.
* An ISP AVR Programmer that is supported by avrdude.
* [面包版](http://www.jameco.com/webapp/wcs/stores/servlet/Product_10001_10001_20723_-1?avad=74941_d4bd4661&source=Avantlink)
* [连接线](http://www.avantlink.com/click.php?tt=cl&mi=10609&pw=74941&url=http%3A%2F%2Fwww.jameco.com%2Fwebapp%2Fwcs%2Fstores%2Fservlet%2FStoreCatalogDisplay%3FstoreId%3D10001%26catalogId%3D10001)
* [10kΩ电阻](http://www.avantlink.com/click.php?tt=cl&mi=10609&pw=74941&url=http%3A%2F%2Fwww.jameco.com%2Fwebapp%2Fwcs%2Fstores%2Fservlet%2FProduct_10001_10001_691104_-1)*1, [470Ω电阻](http://www.avantlink.com/click.php?tt=cl&mi=10609&pw=74941&url=http%3A%2F%2Fwww.jameco.com%2Fwebapp%2Fwcs%2Fstores%2Fservlet%2FProduct_10001_10001_690785_-1)*1
* 0.1uF电容*1
* LED*1

## Wiring The Circuit 画电路图
[[http://static.micahcarrick.com/media/images/avr-tutorial/blink-led/schematic.png]]
## Powering The Circuit 
## Compiling the C Program 编译c程序
## The Blink LED Program 闪烁的LED程序
## Programming the AVR Microcontroller AVR单片机编程
## Connecting the AVR Programmer 连接编程器
## Troubleshooting 故障排除