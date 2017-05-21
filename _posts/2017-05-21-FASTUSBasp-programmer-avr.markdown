---
layout: post
title:  "[STM32]: FASTUSBasp programmer for AVR"
date:   2017-05-21 18:33:05 +0900
categories: STM32
---

### Introduction

[STM32F103C8T6][stm32-link] is a nice cheap board which have plenty of 
features especially USB2.0 full-speed and CAN support. I have loved Atmel AVR  microcontrollers and used legendary [USBasp][usbasp-link] programmer to program.  

  [USBasp programmer][usbasp-link] is a great programmer and uses firmware-only USB driver. I wanted to create another AVR programmer which is similar to USBasp but with 
 real USB hardware to avoid connection issues which I had with USBasp before.
 The price of this board is just `1.81$` in case you buy it from [Aliexpress][stm32-link] hence it was a good alternative to USBasp.
 
### How to compile and upload the firmware

#### compile  

Refer my old post on how to setup stm32 devlopment environment on Arch linux.
[setting up stm32 development environment][stm32dev-link]

Make sure you have compiled `libopencm3` library.

```shell
$ git clone https://github.com/amitesh-singh/FASTUSBasp
$ vi config.cmake  # set the libopencm3 path here
$ cmake .
$ make
```

#### Upload the firmware  

connect `st-link` programmer to `blue-pill` and upload the firmware

```shell
$ make fastusbasp-upload
```

### How to program AVR MCUs

#### Probe AVR

```shell
$ sudo avrdude -c usbasp-clone -p m16

avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9403 (probably m16)

avrdude: safemode: Fuses OK (E:FF, H:D8, L:CF)

avrdude done.  Thank you.
```

#### Upload program to AVR

```shell
$ sudo avrdude -c usbasp-clone -p m16 -U flash:w:blink.hex 

avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9403 (probably m16)
avrdude: NOTE: "flash" memory has been specified, an erase cycle will be performed
         To disable this feature, specify the -D option.
avrdude: erasing chip
avrdude: reading input file "blink.hex"
avrdude: input file blink.hex auto detected as Intel Hex
avrdude: writing flash (150 bytes):

Writing | ################################################## | 100% 0.02s

avrdude: 150 bytes of flash written
avrdude: verifying flash memory against blink.hex:
avrdude: load data flash data from input file blink.hex:
avrdude: input file blink.hex auto detected as Intel Hex
avrdude: input file blink.hex contains 150 bytes
avrdude: reading on-chip flash data:

Reading | ################################################## | 100% 0.01s

avrdude: verifying ...
avrdude: 150 bytes of flash verified

avrdude: safemode: Fuses OK (E:FF, H:D8, L:CF)

avrdude done.  Thank you.
```

#### Read flash

```shell
$ sudo avrdude -c usbasp-clone -p m16 -U flash:r:flash.bin:r

avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9403 (probably m16)
avrdude: reading flash memory:

Reading | ################################################## | 100% 0.33s

avrdude: writing output file "flash.bin"

avrdude: safemode: Fuses OK (E:FF, H:D8, L:CF)

avrdude done.  Thank you.

$ hexdump flash.bin 
0000000 c029 0000 c02f 0000 c02d 0000 c02b 0000
0000010 c029 0000 c027 0000 c025 0000 c023 0000
0000020 c021 0000 c01f 0000 c01d 0000 c01b 0000
0000030 c019 0000 c017 0000 c015 0000 c013 0000
0000040 c011 0000 c00f 0000 c00d 0000 c00b 0000
0000050 c009 0000 2411 be1f e5cf e0d4 bfde bfcd
0000060 d002 c017 cfcd 9ab8 9ac0 ef2f ed83 e390
0000070 5021 4080 4090 f7e1 c000 0000 98c0 ef2f
0000080 ed83 e390 5021 4080 4090 f7e1 c000 0000
0000090 cfeb 94f8 cfff                         
```

#### Flash Read/Write speed  

Flash Write speed: `15 KBps`  
Flash Read Speed: `52.5 KBps`  

#### USB debug information

```shell
$ sudo lsusb -vv -d 16c0:

Bus 003 Device 005: ID 16c0:05dc Van Ooijen Technische Informatica shared ID for use with libusb
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass          255 Vendor Specific Class
  bDeviceSubClass         0 
  bDeviceProtocol         0 
  bMaxPacketSize0        64
  idVendor           0x16c0 Van Ooijen Technische Informatica
  idProduct          0x05dc shared ID for use with libusb
  bcdDevice            2.00
  iManufacturer           1 ami
  iProduct                2 FASTUSBasp
  iSerial                 0 
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength           18
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0 
    bmAttributes         0x80
      (Bus Powered)
    MaxPower              100mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass         0 
      bInterfaceSubClass      0 
      bInterfaceProtocol      0 
      iInterface              0 
can't get device qualifier: Resource temporarily unavailable
can't get debug descriptor: Resource temporarily unavailable
Device Status:     0x0000
  (Bus Powered)
```

#### Adapter board

I have designed three types of PCBs for adapter board. The PCB version 3 can be found at
[PCB v3][pcbv3-link].
There is a panelized PCB which consists of 3 boards in
`100x100 sq. mm` area to save cost. You can download gerber files from
[here][panelizedpcbs-link].

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://t.co/8CcZjQdlFD">https://t.co/8CcZjQdlFD</a><br>Added one more PCB. <a href="https://t.co/7Pehh8KH1r">pic.twitter.com/7Pehh8KH1r</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/866188489456467969">May 21, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I will be ordering panel PCBs soon.
You can follow this project on [Hackaday][hackaday-link].

#### TODOs

##### firmware

- support more `SCK` options.
- support `TPI`.

[stm32-link]: https://www.aliexpress.com/item/Arm-cortex-m3-stm32f103c8t6-stm32-core-board-development-board/1539984258.html
[ali-link]: http://aliexpress.com
[stlinkv2-link]: https://www.aliexpress.com/item/Free-shipping-Smart-Electronics-ST-LINK-Stlink-ST-Link-V2-Mini-STM8-STM32-Simulator-Download-Programmer/32756146997.html
[stmpage-link]: http://www.st.com/en/microcontrollers/stm32f103c8.html
[pcbv3-link]: https://github.com/amitesh-singh/FASTUSBasp/tree/master/kicad3
[stm32dev-link]: http://amitesh-singh.github.io/stm32/2017/04/09/setting-stm32-dev-environment-arch-linux.html
[panelizedpcbs-link]: https://github.com/amitesh-singh/FASTUSBasp/blob/master/kicad3/pan-gerber/panelizedv02.zip?raw=true
[usbasp-link]: http://www.fischl.de/usbasp/
[hackaday-link]: https://hackaday.io/project/21189-fastusbasp-programmer-for-avr
