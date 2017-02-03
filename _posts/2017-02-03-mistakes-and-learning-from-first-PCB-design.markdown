---
layout: post
title:  "Mistakes and Learning from First PCB"
date:   2017-02-03 23:08:36 +0530
categories: PCB
---

In [previous post][pcb-order-link], I have mentioned about my first PCB order to [LionCircuits][lioncircuits-link].
I could have designed PCB better but its okay since Its my first PCB design. 
You learn from your mistakes.

### Mistakes & Learning

I did following mistakes.

-  RST connection circuit: 
   I put switch so that I could reset AVR microcontroller by pressing switch button.
   I could have done it better.

   ![wrong-circuit](/images/rstwrongcircuit.png)

   The above circuit will work fine in most of the cases since RST pin has internal pullup but 
   its weak one.
   In case of noisy environment, AVR MCU might get reset so to avoid that, you need to provide
   external pullups like below.
   ![rst-circuit](/images/rstcorrectcircuit.png)

- 90 degree angle traces at RST circuit: Ideally we should avoid it although RST circuit is 
    not used for data transfer so I guess its fine.
    For more information why 90 degree angle traces are bad, check this link [90 degree PCB][degree-pcb].
    ![trace-wrong](/images/tracewrong.png)
-  Orientation of label texts on ISP pins.

Next time, I would verify PCBs by [PCB checklist][pcbchecklist-link]. 

Happy Learning!

[degree-pcb]: http://electronics.stackexchange.com/questions/226582/pcb-90-degree-angles
[pcb-order-link]: http://amitesh-singh.github.io/pcb/2017/02/02/creating-cheap-circuit-boards.html
[kicad-link]: http://kicad-pcb.org
[lioncircuits-link]: http://lioncircuits.com
[board-link]: https://github.com/amitesh-singh/kicad-pcbs/tree/master/kicad/attiny13aRstbutton
[pcbchecklist-link]: http://grizzlypeak.io/notes/pcb-checklist.html