---
layout: post
title:  "[STM32]: Overclocking Blue Pill boards"
date:   2018-06-17 10:54:00 +0530
categories: STM32
---

Overclocking STM32 is far easier than overclocking AVR. In AVR, in order to overclock, high speed crystal of that required speed is needed. Also you need to compile the code prior with the overclocked MCU speed to generate hex file. This sounds a lot of work and ofcourse, you can't change the AVR speed dynamically at run time. In STM32, overclocking can be done with a simple change in code: `Change the PLL settings`.

Blue pill board has an external 8Mhz crystal, and it is used as the external clock (HSE). All you have to do is to change the PLL multiplier to 16 (maximum value) which results into overclocking STM32F103 to `8MHZ X 16 = 128 MHz`. 128MHz is a blistering MCU speed for a cost of `< 2$`. :)

### Show me the Code

Without any further delay, below is the code for setting stm32 clock to 128Mhz. This code is based on popular open source library `libopencm3`.

 {% highlight C %}
void rcc_clock_setup_in_hse_8mhz_out_128mhz()
{
   /* Enable internal high-speed oscillator. */
   rcc_osc_on(RCC_HSI);
   rcc_wait_for_osc_ready(RCC_HSI);

   /* Select HSI as SYSCLK source. */
   rcc_set_sysclk_source(RCC_CFGR_SW_SYSCLKSEL_HSICLK);

   /* Enable external high-speed oscillator 8MHz. */
   rcc_osc_on(RCC_HSE);
   rcc_wait_for_osc_ready(RCC_HSE);
   rcc_set_sysclk_source(RCC_CFGR_SW_SYSCLKSEL_HSECLK);

   /*
    * Set prescalers for AHB, ADC, ABP1, ABP2.
    * Do this before touching the PLL (TODO: why?).
    */
   rcc_set_hpre(RCC_CFGR_HPRE_SYSCLK_NODIV);    /* set it to 128MHz */
   rcc_set_adcpre(RCC_CFGR_ADCPRE_PCLK2_DIV8);  /* 128/8 = 16Mhz, Max. 14MHz */
   rcc_set_ppre1(RCC_CFGR_PPRE1_HCLK_DIV2); // 128/2 = 64MHz, Max: 36MHz
   rcc_set_ppre2(RCC_CFGR_PPRE2_HCLK_NODIV);    /* Set. 128MHz Max. 72MHz */
   
   // Disable USB since it won't work at overclocked STM32
   // rcc_set_usbpre(RCC_CFGR_USBPRE_PLL_CLK_NODIV); /* Set 48Mhz, Max: 48Mhz */

   /*
    * Sysclk runs with 24MHz -> 0 waitstates.
    * 0WS from 0-24MHz
    * 1WS from 24-48MHz
    * 2WS from 48-72MHz
    */
   flash_set_ws(FLASH_ACR_LATENCY_2WS);

   /*
    * Set the PLL multiplication factor to 16.
    * 8MHz (external) * 16 (multiplier) = 128MHz
    */
   rcc_set_pll_multiplication_factor(RCC_CFGR_PLLMUL_PLL_CLK_MUL16);

   /* Select HSE as PLL source. */
   rcc_set_pll_source(RCC_CFGR_PLLSRC_HSE_CLK);

   /*
    * External frequency undivided before entering PLL
    * (only valid/needed for HSE).
    */
   rcc_set_pllxtpre(RCC_CFGR_PLLXTPRE_HSE_CLK);
  /* Enable PLL oscillator and wait for it to stabilize. */
   rcc_osc_on(RCC_PLL);
   rcc_wait_for_osc_ready(RCC_PLL);

   /* Select PLL as SYSCLK source. */
   rcc_set_sysclk_source(RCC_CFGR_SW_SYSCLKSEL_PLLCLK);

   /* Set the peripheral clock frequencies used */
   rcc_ahb_frequency = 128000000;
   rcc_apb1_frequency = 64000000;
   rcc_apb2_frequency = 128000000;
}
 {% endhighlight %}

The drawback with overclocking the STM32 is that USB will only work when the clock runs at 72Mhz or 48Mhz, as the F103 has only two USB clock divider settings.
