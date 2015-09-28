http://blog.codylab.com/thinkpad-thinkfan/


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