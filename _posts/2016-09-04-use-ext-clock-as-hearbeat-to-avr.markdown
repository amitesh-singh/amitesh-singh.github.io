---
layout: post
title:  "Use external clock as heartbeat to AVR and overclocking"
date:   2016-09-04 11:08:36 +0530
categories: AVR
---

Hello Everyone,

Recently I have ordered few samples from [Linear Technologies][lt-link] which includes

1. [LTC1799][lt1799-link]
2. [LTC6900][lt6900-link]

I thank [LT India][lt-link] to provide these awesome oscillators to me. I received samples
from LT India, Banglore. I needed these to overclock AVR microcontrollers.

### Soldering
These chips are in TSOT23-5 pkg and I wanted them to try on breadboard first.
These ICs are really tiny.

![LTC1799 TSOT23-5 size](/images/lt1799.jpg)

Luckily I had few TSOT23-8 to DIP adapters, so I soldered them nicely on these adapter boards.
If you are new to smd soldering, I prefer you to check youtube videos on this topic.

![LTC1799/6900 soldered on DIP adapters](/images/lt1799-soldered.jpeg)

### Advantages of using LTC1799

1. It only uses `XTAL1` pin of avr.
2. `1KHz to 33 MHz` Frequency Range.
3. Facilitates experimentation on testing lowest power consumption design on low speed. (say 10Khz or lesser)

### Setup

#### Fuse settings

You have to change fuse settings of avr (attiny85 in our case).
I used [avr fuse calculator][avrfusecalc-link] to find the correct settings for attiny85.

{% highlight bash %}
$ avrdude -c usbasp -p t85 -U lfuse:w:0xe0:m -U hfuse:w:0xdc:m -U efuse:w:0xff:m
{% endhighlight %}

#### Connections
The port 5 of LTC1799 must be connected to attiny85 XTAL1 pin.
I used R_set to 3.25k&\#8486; so that I get 30 MHz clock from oscillator. My main aim was to
test attiny85 running at 30 MHz. AVR attiny85 Datasheet recommends to run it up to 20 MHz only.
Refer [LTC1799 datasheet][ltc1799ds-link] for the formula to program LTC1799. Its fairly easy though.

{% highlight python %}

f_osc = 10MHz*((10k)/(N*R))
                          where N = 1 and R = 3.25K

f_osc = 10 * 1000000 * 10 * 1000 / (1 * 3.25 * 1000)
      = 30769230.769230768 Hz
 {% endhighlight %}

![LT1799 schematic](/images/4977.png)
Reference: [LTC1799 schematic][lt1799sch-link]
![Attiny85 schematic](/images/attiny85_powerup-ext-clock_schem.png)
Running class blink program at 30 MHz. ![Runing classic blink at 30 MHz](/images/attiny85_ltc1799_final.jpg)

### Verrifying with avrdude

{% highlight bash %}
$ avrdude -c usbasp -p t85

avrdude: warning: cannot set sck period. please check for usbasp firmware update.
avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.02s

avrdude: Device signature = 0x1e930b (probably t85)

avrdude: safemode: Fuses OK (E:FF, H:DF, L:E0)

avrdude done.  Thank you.
 {% endhighlight %}

[avrfusecalc-link]: http://www.engbedded.com/fusecalc/
[lt-link]: http://www.linear.com
[lt1799-link]: http://www.linear.com/product/LTC1799
[lt6900-link]: http://www.linear.com/product/LTC6900
[lt1799sch-link]: http://cds.linear.com/image/4977.png
[ltc1799ds-link]: http://cds.linear.com/docs/en/datasheet/1799fd.pdf
