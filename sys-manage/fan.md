#控制风扇

环境: debian t430


参考1

sudo apt-get install thinkfan


sudo echo "options thinkpad_acpi fan_control=1" | sudo tee /etc/modprobe.d/thinkpad_acpi.conf

有这句话就行


cat /proc/acpi/ibm/fan
status:     enabled
speed:      4464
level:      auto
commands:   level <level> (<level> is 0-7, auto, disengaged, full-speed)
commands:   enable, disable
commands:   watchdog <timeout> (<timeout> is 0 (off), 1-120 (seconds))

有`commands`行

echo level 0 | sudo tee /proc/acpi/ibm/fan

echo level auto | sudo tee /proc/acpi/ibm/fan



简单命令

这里开始参考2

```
# 控制风扇
function fan() {
  sensors
  if [ $# -eq 1 ] ; then
    echo level $@ | sudo tee /proc/acpi/ibm/fan
  fi
}
```

温度控制

注意关键字 hwmon
```
sudo pluma /etc/thinkfan.conf
```
```
tp_fan /proc/acpi/ibm/fan
# hwmon /sys/devices/platform/coretemp.0/hwmon/hwmon3/temp1_input

hwmon /sys/devices/virtual/hwmon/hwmon0/temp1_input
hwmon /sys/devices/platform/coretemp.0/hwmon/hwmon3/temp3_input 
hwmon /sys/devices/platform/coretemp.0/hwmon/hwmon3/temp1_input
hwmon /sys/devices/platform/coretemp.0/hwmon/hwmon3/temp2_input

(0,	0,	60)
(1,	55,	65)
(2,	60,	69)
(3,	65,	70)
(4,	66,	71)
(5,	67,	72)
(6,	68,	73)
(7,	67,	32767)
```

# 参考
1. http://blog.codylab.com/thinkpad-thinkfan/
2. http://blog.mixu.net/2012/01/19/how-to-thinkpad_acpi-and-fan-control-on-arch/