---
layout: post
title:  "[STM32]: Setting STM32 development environment on Arch Linux - Part 1"
date:   2017-04-09 23:33:05 +0530
categories: STM32
---

I have bought [STM32F103C8T6][stm32-link] aka "Blue-Pill" and [ST Link V2][stlinkv2-link] from [Aliexpress][ali-link] long time back. I did not get time earlier to explore this MCU. The price of this board is just `1.81$` in case you buy it from [Aliexpress][stm32-link].

#### Key Features

- ARM® 32-bit Cortex® -M3 CPU Core  
        72 MHz maximum frequency,1.25 DMIPS/MHz (Dhrystone 2.1) performance at wait state memory access  
        Single-cycle multiplication and hardware division  
- Memories  
        64 or 128 Kbytes of Flash memory  
        20 Kbytes of SRAM  
- Clock, reset and supply management  
        2.0 to 3.6 V application supply and I/Os  
        POR, PDR, and programmable voltage detector (PVD)  
        4-to-16 MHz crystal oscillator  
        Internal 8 MHz factory-trimmed RC  
        Internal 40 kHz RC  
        PLL for CPU clock  
        32 kHz oscillator for RTC with calibration  
 - Low-power  
        Sleep, Stop and Standby modes  
        VBAT supply for RTC and backup registers  
- 2 x 12-bit, 1 μs A/D converters (up to 16 channels)  
        Conversion range: 0 to 3.6 V  
        Dual-sample and hold capability  
        Temperature sensor  
- DMA  
        7-channel DMA controller  
        Peripherals supported: timers, ADC, SPIs, I2 Cs and USARTs  
- Up to 80 fast I/O ports  
        26/37/51/80 I/Os, all mappable on 16 external interrupt vectors and almost all 5 V-tolerant  
- Debug mode  
        Serial wire debug (SWD) & JTAG interfaces  
 - 7 timers  
        Three 16-bit timers, each with up to 4 IC/OC/PWM or pulse counter and quadrature (incremental) encoder input  
        16-bit, motor control PWM timer with dead-time generation and emergency stop  
        2 watchdog timers (Independent and Window)  
        SysTick timer 24-bit downcounter  
- Up to 9 communication interfaces  
        Up to 2 x I2 C interfaces (SMBus/PMBus)  
        Up to 3 USARTs (ISO 7816 interface, LIN, IrDA capability, modem control)  
        Up to 2 SPIs (18 Mbit/s)  
        CAN interface (2.0B Active)  
        USB 2.0 full-speed interface  
- CRC calculation unit, 96-bit unique ID    

For more information, go to [STMicroelectronics product page][stmpage-link]
### Install gcc and newlib

{% highlight C %}
$ sudo pacman -S arm-none-eabi-gcc arm-none-eabi-newlib
{% endhighlight %}

### Install stlink and openocd

stlink is a tool to download the compiled binary to the microcontroller
and OpenOCD is an on-chip debugger. I shall cover OpenOCD in future tutorials.

{% highlight C %}
$ sudo pacman -S stlink openocd
{% endhighlight %}

### install libopencm3
Libopencm3 is a nice opensource `C` library for cortex M0/M1/M3 and other MCUs. 

{% highlight C %}
$ git clone https://github.com/libopencm3/libopencm3
$ cd libopencm3
$ make

{% endhighlight %}

This should compile without any issue. 

### Write your blink LED sketch

The onboard LED on board is connect at port `GPIO13` of `GPIOC`.
`gpio_toggle()` is the function to toggle the state of GPIOs and we would be using this function to blink the onboard led.

{% highlight C %}

#include <libopencm3/cm3/common.h>
#include <libopencm3/stm32/rcc.h>
#include <libopencm3/stm32/gpio.h>

static void my_delay_1( void )
{
   int i = 72e6/2/4;

   while( i > 0 )
     {
        i--;
        __asm__( "nop" );
     }
}

int main( void )
{
   //set STM32 to 72 MHz
   rcc_clock_setup_in_hse_8mhz_out_72mhz();

   // Enable GPIOC clock
   rcc_periph_clock_enable(RCC_GPIOC);

   //Set GPIO13 (inbuild led connected) to 'output push-pull'
   gpio_set_mode(GPIOC, GPIO_MODE_OUTPUT_2_MHZ, GPIO_CNF_OUTPUT_PUSHPULL,
                 GPIO13);

   while(1)
     {
        gpio_toggle(GPIOC, GPIO13);
        my_delay_1();
     }
}

{% endhighlight %}

`svn` command can be used to download complete blink sample code.

{% highlight C %}
$ svn export https://github.com/amitesh-singh/amiduino/trunk/stm32/f1/blink
A    blink
A    blink/Makefile
A    blink/hello.c
A    blink/hello.ld
A    blink/libopencm3-examples-rules.mk
A    blink/libopencm3-examples-stm32f1.mk
A    blink/notes.txt
A    blink/stm32f103c8t6_board_schema.pdf
Exported revision 367.


$ cd blink
$ ls
hello.c   libopencm3-examples-rules.mk    Makefile   stm32f103c8t6_board_schema.pdf
hello.ld  libopencm3-examples-stm32f1.mk  notes.txt

{% endhighlight %}

You might need to modify the `libopencm3` repo path in `libopencm3-examples-rules.mk`. In my case, it is set to `$(HOME)/repos/libopencm3.`

{% highlight C %}
libopencm3-examples-rules.mk                                                   
 ...

 ###############################################################################
 # Source files
 
 OBJS     += $(BINARY).o
 
 
 ifeq ($(strip $(OPENCM3_DIR)),)
 # user has not specified the library path, so we try to detect it
 
 # where we search for the library
 #LIBPATHS := ./libopencm3 ../../../../libopencm3 ../../../../../libopencm3
 LIBPATHS := $(HOME)/repos/libopencm3
 ..
 ..
{% endhighlight %}

Build the blink program.

{% highlight C %}

$ make
  CC      skeleton.c
  LD      skeleton.elf
  OBJCOPY skeleton.bin
{% endhighlight %}

### Uploading the program

You will need [ST Link V2][stlinkv2-link] programmer to upload the code. 

{% highlight C %}
$ st-flash write skeleton.bin 0x08000000
st-flash 1.3.1
2017-04-10T00:12:55 INFO src/usb.c: -- exit_dfu_mode
2017-04-10T00:12:55 INFO src/common.c: Loading device parameters....
2017-04-10T00:12:55 INFO src/common.c: Device connected is: F1 Medium-density device, id 0x20036410
2017-04-10T00:12:55 INFO src/common.c: SRAM size: 0x5000 bytes (20 KiB), Flash: 0x10000 bytes (64 KiB) in pages of 1024 bytes
2017-04-10T00:12:55 INFO src/common.c: Attempting to write 1236 (0x4d4) bytes to stm32 address: 134217728 (0x8000000)
Flash page at addr: 0x08000400 erased
2017-04-10T00:12:55 INFO src/common.c: Finished erasing 2 pages of 1024 (0x400) bytes
2017-04-10T00:12:55 INFO src/common.c: Starting Flash write for VL/F0/F3 core id
2017-04-10T00:12:55 INFO src/flash_loader.c: Successfully loaded flash loader in sram
  1/1 pages written
2017-04-10T00:12:55 INFO src/common.c: Starting verification of write complete
2017-04-10T00:12:55 INFO src/common.c: Flash written and verified! jolly good!

{% endhighlight %}

You should see GREEN LED blinking. 

<blockquote class="twitter-video" data-lang="en"><p lang="en" dir="ltr">Blink led on stm32 works. <a href="https://twitter.com/hashtag/stm32?src=hash">#stm32</a> <a href="https://t.co/NfrgWu2dmr">pic.twitter.com/NfrgWu2dmr</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/834101989499949057">February 21, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>



[stm32-link]: https://www.aliexpress.com/item/Arm-cortex-m3-stm32f103c8t6-stm32-core-board-development-board/1539984258.html
[ali-link]: http://aliexpress.com
[stlinkv2-link]: https://www.aliexpress.com/item/Free-shipping-Smart-Electronics-ST-LINK-Stlink-ST-Link-V2-Mini-STM8-STM32-Simulator-Download-Programmer/32756146997.html
[stmpage-link]: http://www.st.com/en/microcontrollers/stm32f103c8.html
