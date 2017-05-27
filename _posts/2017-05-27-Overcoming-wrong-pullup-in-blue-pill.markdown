---
layout: post
title:  "[STM32]: Overcoming wrong pullup resistor at D+ in blue pill"
date:   2017-05-27 22:10:10 +0900
categories: STM32
---

### Introduction

We all love [STM32F103C8T6][stm32-link] aka `blue-pill` since its a nice cheap board (`1.81$` from [Aliexpress][ali-link]) which have plenty of 
features especially USB2.0 full-speed and CAN support.
Unfortunately there is a hardware problem with chinese board at USB port.

### Wrong USB pullup resistor at D+ line

I was more interested into USB2.0 support and tried a simplest usb program based on
[libopencm3][libopencm3-link] but it did not work.
I googled a bit and found following link. 

[http://wiki.stm32duino.com/index.php?title=Blue_Pill][bluepill-link]

![bluepull resistor](/images/BluePillUsbResistor.jpg)
> Hardware installation
The USB standard require a 1.5k pullup resistor on D+, but this board is known to have a wrong value (R10 on the board). It ships with either a 10K resistor or a 4.7k resistor, but it should be replaced with a 1.5k resistor, or put an appropriate resistor value (e.g 1.8k) in between PA12 and 3.3V. It is also true that some PCs are tolerant of incorrect value so, before you change the resistance, you can try if it works in your case.

### Solution

I did not want to replace the `R10` with a 1.5k resistor and I was looking 
for some software hack to overcome this defect.
The below code is a hack to trigger device enumeration at startup.
Driving D+ pin to low explicitly removes the pullup effectively and subsequent `usb_init()` call
will take over the pin and it will appear as proper pullup to the host.

The delay is arbitary and 5 ms delay works almost all the time. 
{% highlight C %}
//This is required if proper pullup is not present at D+ line.
// This is must for chinese stm32f103c8t6 aka "blue pill"
rcc_periph_clock_enable(RCC_GPIOA);
gpio_set_mode(GPIOA, GPIO_MODE_OUTPUT_2_MHZ,
              GPIO_CNF_OUTPUT_PUSHPULL, GPIO12);
gpio_clear(GPIOA, GPIO12);
msleep(5); //delay
{% endhighlight %}

[bluepill-link]: http://wiki.stm32duino.com/index.php?title=Blue_Pill
[stm32-link]:https://www.aliexpress.com/item/Arm-cortex-m3-stm32f103c8t6-stm32-core-board-development-board/1539984258.html
[ali-link]: http://aliexpress.com
[libopencm3-link]: https://github.com/libopencm3/libopencm3