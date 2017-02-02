---
layout: post
title:  "Creating Cheap PCBs In India for Makers: Designing and ordering, Part 1"
date:   2016-02-02 11:08:36 +0530
categories: PCB
---

Most of the Indian elecronics hobbyists think that a PCB fabrication is expensive and
time consuming process. I thought that too. My requirements were 

1. `cheap prototyping boards`
2. `Faster shipping` 

I am glad that I found [LionCircuits][lioncircuits-link], an indian startup, based in banglore,
which proudly makes PCBs in India for Makers and it meets my requirements. I am happy of the fact 
this company supports makers community since most of us don't like soldering on perf board although I 
do good job on soldering perf board. Its just not worth the effort you put to make a circuit on perf board.

Do you want proof of my perf board soldering skills? :)


![perb board solder bridges](/images/perf-bridges.jpg)

### PCB

There are so many ways to make a PCB. You can itch a PCB at home but I don't like it and its messy,
unreliable and not worth the effort. Also It involves hazardous chemicals so you have to be careful
 while doing it.
You can use [Voltera circuit prototyping machine][voltera-link] but its costly and I am not sure if 
you can get this locally without spending few more dollars on customs and Shipping or You can just
 get your PCB design manufactured by a professional manufacturer like [LionCircuits][lioncircuits-link]
for the price of half KFC bucket or Pizza. I prefer this one.

### Kicad
[KiCad][kicad-link] is a free software suite for electronic design automation (EDA).
 It facilitates the design of schematics for electronic circuits and their conversion to PCB designs.
 It is opensource and there are no limitations and you can do wonders in it. :)

#### Learning
It took me 2 days to learn most of it. I have started with [Contextual Electronics][chris-link]
and then move to other youtube videos and so on.
Also the guidance by [Martin aka sabor][martin-blog-link] has been immense. He is an expert in
Electronics and doing it for last 20 years or so. Check out his [panelized script][panel-link] which is really helpful.

### LionCircuits
For PCB prototyping, LionCircuits charges `INR 36/sq.cm` for `three two Layer boards` and ships `under 14 days` which includes
 `FREE Shipping` too. All made in India. :) 

 I have paid `240/- INR` for my first 2 layer board attiny13a breakout board of size 2x4 sq cm.
 You can find kicad project here: [attiny13a board kicad link][board-link]

#### 3D VIEW
![Kicad pcb](/images/board-pcb.png)
![Top View](/images/t13aboard-top.png)
![Bottom View](/images/t13aboard-bottom.png)

### Uploading gerber
The user interface for uploading gerber files is nice and I was able to upload gerber without any
issues. To upload gerber files, just click on "upload design" link on [LionCircuits][lioncircuits-link]
and register and place the order. They are working on improving the user interface. 
You can send your gerber files to `sales@lioncircuits.com` as well.

### Support and Excitement
The support from [LionCircuits][lioncircuits-link] has been fantastic so far and they have replied
to each query. It has been pleasure and I am looking forward to order more PCBs from them. 
[LionCircuits][lioncircuits-link] is a gift to Indian Electronics Maker community. Its cheap and fastest
way to get your PCB done. 

This is my first PCB order and I am pretty excited about it. I have done some mistakes in my 
pcb design but this is the way you learn.

I will do the review of the boards when I get it. Stay tuned for second part of this article.

[kicad-link]: http://kicad-pcb.org
[lioncircuits-link]: http://lioncircuits.com
[voltera-link]: https://www.kickstarter.com/projects/voltera/voltera-your-circuit-board-prototyping-machine
[board-link]: https://github.com/amitesh-singh/kicad-pcbs/tree/master/kicad/attiny13aRstbutton
[martin-blog-link]: http://blog.borg.ch/
[chris-link]: https://contextualelectronics.com/
[panel-link]: http://blog.borg.ch/?p=12