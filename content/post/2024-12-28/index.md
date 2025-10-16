---
title: RockPI 4c+ fan control
description: Controlling the fan included with RockPI 4c+
slug: rockpi-4c-fan-control
date: 2024-12-28 00:00:00+0000
image: cover.png
categories:
    - rpi
    - hardware
tags:
    - raspberrypi
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---
# RockPI 4c+ fan control

```bash
   echo 0 > /sys/class/pwm/pwmchip0/export
   echo 40000 > /sys/class/pwm/pwmchip0/pwm0/period
   echo 10000 > /sys/class/pwm/pwmchip0/pwm0/duty_cycle
   echo normal >  /sys/class/pwm/pwmchip0/pwm0/polarity
   echo 1 > /sys/class/pwm/pwmchip0/pwm0/enable
   echo 1 > /sys/class/pwm/pwmchip0/power/runtime_enabled
   /sys/class/pwm/pwmchip0/device/power/
   apt-get install -y rockpi4-dtbo
   cat /boot/hw_intfc.conf
   rsetup
   apt search coolero
   apt search lm-sensor
   apt install lm-sensors
   sensors -v
   sensors-detect
   pwmconfig
   apt install fancontrol
   pwmconfig
   echo 0 > /sys/class/pwm/pwmchip0/export
   echo 40000 > /sys/class/pwm/pwmchip0/pwm0/period
   echo 10000 > /sys/class/pwm/pwmchip0/pwm0/duty_cycle
   echo normal >  /sys/class/pwm/pwmchip0/pwm0/polarity
   ls -l /sys/class/pwm/pwmchip0/device/pwm/pwmchip0/
   echo 0 > /sys/class/pwm/pwmchip0/device/pwm/pwmchip0/export
   echo 1 > /sys/class/pwm/pwmchip0/pwm0/enable

   echo 0 > /sys/class/pwm/pwmchip2/export
   echo 40000 > /sys/class/pwm/pwmchip2/pwm0/
   echo 10000 > /sys/class/pwm/pwmchip2/pwm0/duty_cycle
   echo normal >  /sys/class/pwm/pwmchip2/pwm0/polarity
   echo 0 > /sys/class/pwm/pwmchip2/device/pwm/pwmchip0/export
   echo 1 > /sys/class/pwm/pwmchip2/pwm0/enable
```