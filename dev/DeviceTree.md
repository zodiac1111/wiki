# 设备树

# 参考资料

* 官方 http://www.devicetree.org/Main_Page
* 树结构,节点和属性 http://devicetree.org/Device_Tree_Usage
* 简介 https://learn.adafruit.com/introduction-to-the-beaglebone-black-device-tree
* pin定义 https://www.kernel.org/doc/Documentation/devicetree/bindings/pinctrl/pinctrl-single.txt

```text
/linux-3.14.2/drivers/pinctrl 这里看上去也是具体厂商的各种实现文件..
/drivers/pinctrl/sirf/pinctrl-atlas6.c:999:	SIRFSOC_PMX_FUNCTION("uart1", uart1grp, uart1_padmux),
./drivers/pinctrl/pinctrl-tz1090.c:536:	"uart1",
./drivers/pinctrl/pinctrl-adi2-bf60x.c:481:	ADI_PMX_FUNCTION("uart1", uart1grp, uart1_mux),
看上去各种宏定义和数组把字符串和具体的地址关联在了一起
./Documentation/devicetree/bindings/pinctrl/pinctrl_spear.txt:134:
```