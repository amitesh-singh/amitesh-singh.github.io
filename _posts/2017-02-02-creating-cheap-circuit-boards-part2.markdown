---
layout: post
title:  "Creating Cheap PCBs In India for Makers: Soldering and Review, Part 2"
date:   2017-02-12 12:15:36 +0530
categories: PCB
---

In previous post [Creating Cheap PCBs In India for Makers: Designing and ordering, Part 1][prevpost-link], we have discussed about how to design PCB and finally 
how to order the pcbs from [LionCircuits][lioncircuits-link].
I have ordered my PCBs on `January 30 2017` and my PCBs were shipped on `Feb 7 2017` and it reached
to me on `Feb 10 2017`. So it took total `11 days` which is quite FAST if you compare it with other
PCBs manufacturers. Also it was a nice gesture from [LionsCircuit][lioncircuits-link] to give me 
`1000/- INR` bonus. :)
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Thank you <a href="https://twitter.com/LionCircuits">@LionCircuits</a> for these jewels and bonus. ;)<br>Can&#39;t wait to solder these babies and try. Reviews  soon on my blog. 👍 <a href="https://t.co/u0gw1OGQAW">pic.twitter.com/u0gw1OGQAW</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/830307470815985664">February 11, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### soldering 
I don't have solder paste and hot air gun so I was totally relying on my solder iron and my skill to solder
this. Two boards turn out to be okay but I screwed one board while soldering because I put too much
force on pulling a stucked solder wire on solder pad. :/ Although a fine jumper wire could be used
to save this :). Probably I will do it later when I actually need to.
Also I need to order solder paste and good quality hot air gun now as I would be doing lot of `SMD` soldering
stuffs.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Two out of three turns out okay. ☺Works well. I screwed one pcb by putting force on pulling a stuck solder wire on solder pad. 😠 <a href="https://t.co/VTp2ztD6rv">pic.twitter.com/VTp2ztD6rv</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/830344488862306304">February 11, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Look at those perfect solder joints. Pure gold. <a href="https://t.co/SVwiZAcNLj">pic.twitter.com/SVwiZAcNLj</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/830345924530548736">February 11, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### Testing
The default setting in attiny13a is to use 9.6 MHz internal oscillator.

{% highlight C %}
ami@ami-desktop:~$ avrdude -c usbasp -p t13

avrdude: warning: cannot set sck period. please check for usbasp firmware update.
avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.02s

avrdude: Device signature = 0x1e9007 (probably t13)

avrdude: safemode: Fuses OK (E:FF, H:FF, L:7A)

avrdude done.  Thank you.
{% endhighlight %}
I did upload a blinking LED sketch which worked well. Check the video below.
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Okay. Blinking LEDs works.<a href="https://twitter.com/LionCircuits">@LionCircuits</a> <a href="https://t.co/7ERhAsNk2g">pic.twitter.com/7ERhAsNk2g</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/830352446878838784">February 11, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Added a switch and smd components. <a href="https://t.co/oVsOwm6FAh">pic.twitter.com/oVsOwm6FAh</a></p>&mdash; FAITH + 1 (@amitesh_singh) <a href="https://twitter.com/amitesh_singh/status/832321612485709824">February 16, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### Reviews
Overall I am pretty happy with the quality of PCBs and the turn out time from ordering PCB to actually
get delivered to my door steps. I shall definetly order my PCBs from [LionCircuits][lioncircuits-link]
in future.

[prevpost-link]: http://amitesh-singh.github.io/pcb/2017/02/02/creating-cheap-circuit-boards.html
[lioncircuits-link]: http://lioncircuits.com