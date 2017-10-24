---
layout: post
title:  "[STM32]: Correcting pullup resistor on blue pill board"
date:   2017-10-09 16:33:05 +0830
categories: STM32
---

Recently I have ordered five more [blue pills][stm32-link] from [Aliexpress][ali-link] required for my future projects. Its a great board but as you might know that this has wrong pullup resistor (R10=10k) at USB D+ line. The USB standard requires a 1.5k R pullup on D+.

[The software hack][soft-hack] which we discussed before is a good solution and it works 90% times. It might not work on some usb hub or you would need unplug and plug several times to make usb work.

In this tutorial, we will learn how to correct this mistake by replacing with correct USB pullup resistor on D+ line.

### Requirements

- Hot air gun
- `0603 size 1.5k` resistor

### Remove R10
You would need a hot air gun to remove `R10` resistor. Its much easier to remove `R10` by hot air gun.

![wrong R10](https://pbs.twimg.com/media/DLrgTQtUEAAmxz-.jpg)
### replace R10 with 1.5k Resistor
I did not have `0603` size 1.5k resistors. I did salvage 2.2k resistor from a broken step up voltage converter.
You can put some solder on R10 solder pads and solder new resistor on R10. 

![corrected pullup](https://pbs.twimg.com/media/DLrgVpIUMAAgKcn.jpg)

Finally I got 1.5K resistors of size `0603` and fixed remaining four blue pills with it.


[stm32-link]: https://www.aliexpress.com/item/STM32F103C8T6-ARM-STM32-Minimum-System-Development-Board-Module-For-Arduin0/32345958001.html
[soft-hack]: http://amitesh-singh.github.io/stm32/2017/05/27/Overcoming-wrong-pullup-in-blue-pill.html
[ali-link]: http://aliexpress.com
