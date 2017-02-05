---
layout: post
title:  "Setting up second monitor on ubuntu 16.04"
date:   2017-02-05 23:00:39 +0530
categories: Monitor
---

I have bought a second monitor [Samsung 24 inch AH-IPS Led HDMI Monitor LS24E360HL/XL Glossy White][monitor-link] from [amazon india][amazon-link] for home. I already have two monitors at work and I am sure 
of the fact that dual displays improves the productivity. This monitor color is glossy white and
its really more than a monitor with stunning looks. Its carefully designed to keep the focus on your
content viewing pleasure. 


### Setting up second monitor on Xenial

I have connected new monitor via `hdmi` and old monitor (24 inch) via VGA to Desktop.  
There was some trouble in setting up second display (old one) initially on [Ubuntu Xenial][xenial-link].
 The second display resolution was low and there was no other resolution options (e.g. `1980x1080`)
 available.


To correct this, I have added following option in `~/.xprofile` and rebooted the machine.

{% highlight C %}
cvt 1920 1080
xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
xrandr --addmode VGA1 "1920x1080_60.00"
xrandr --output VGA1 --mode "1920x1080_60.00"
{% endhighlight %}

Run xrandr command to view monitors list.
{%highlight bash %}
ami@ami-desktop:~$ xrandr --listmonitors
Monitors: 2
 0: +*HDMI2 1920/521x1080/293+1920+0  HDMI2
 1: +VGA1 1920/508x1080/286+0+0  VGA1
{% endhighlight %}
![myworksetup](/images/mymonitors.jpg)

[monitor-link]: http://www.amazon.in/Samsung-AH-IPS-Monitor-LS24E360HL-XL/dp/B01MFBLREN
[xenial-link]: http://releases.ubuntu.com/16.04/
[amazon-link]: http://amazon.in