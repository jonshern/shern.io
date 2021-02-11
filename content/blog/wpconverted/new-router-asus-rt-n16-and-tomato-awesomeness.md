---
title: 'New Router – Asus RT-N16 and Tomato Awesomeness'
date: Fri, 29 Nov 2013 06:06:51 +0000
draft: false
tags: ['hardware', 'router tomato', 'technology']
---

Jeff Atwood has an excellent article recommending this router.

It can be found [here](http://www.codinghorror.com/blog/2012/06/because-everyone-still-needs-a-router.html).

In the article is a guide to flashing the firmware with tomato.

[http://www.shadowandy.net/2012/03/asus-rt-n66u-tomatousb-firmware-flashing-guide.htm](http://www.shadowandy.net/2012/03/asus-rt-n66u-tomatousb-firmware-flashing-guide.htm)

The one caveat that I would add is.

You have to reset the NVRAM via key presses which means

1.  Power off the [RT-N66U](http://www.amazon.com/RT-N66U-Dual-Band-Wireless-N900-Gigabit-Router/dp/B006QB1RPY?SubscriptionId=AKIAJQ2SU7PDQNTBZPFQ&tag=shadowamylife-20)
2.  Press and hold down the WPS button
3.  While holding the WPS button, plug in the power cable to turn [RT-N66U](http://www.amazon.com/RT-N66U-Dual-Band-Wireless-N900-Gigabit-Router/dp/B006QB1RPY?SubscriptionId=AKIAJQ2SU7PDQNTBZPFQ&tag=shadowamylife-20)on
4.  Keep holding the WPS button for 30 seconds before releasing  
    _The router should reboot_
5.  Congratulations. The NVRAM has been cleared.

I was not able to reach the web address so I was not able to perform the operation via the web admin, so the key press option was my only option.

In performing this process. I found the instructions and distribution of [Easy Tomato](http://www.easytomato.org/) to be fantastic.

[http://www.easytomato.org/installing-easytomato-on-your-router](http://www.easytomato.org/installing-easytomato-on-your-router)

Easy Tomato is by far the best router firmware I have ever used. 

It makes the things that should be easy, easy and allows the advanced settings to be found easily.

**A couple of examples**

You can block any and all adult sites with a single checkbox.(bam)

You can define groups and **drag** the devices on your network into groups.  Yeah I said drag(holy web 2.0 interface batman)

Each group can then have access rules defined for it.

I highly recommend Easy Tomato and the Asus RT-N16 router.