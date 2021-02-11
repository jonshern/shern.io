---
title: 'Recovering data from a computer that won''t boot and has a bad dvd rom'
date: Tue, 04 Dec 2007 04:13:04 +0000
draft: false
tags: ['technology']
---

If the computer will not boot and the dvd drive is bad. You will have to find an alternate way to boot the computer. I used a thumbdrive since the bios supported it. [First get the thumbdrive bootable.](http://www.pendrivelinux.com/2007/01/02/all-in-one-usb-dsl) In order to get access to the windows drives you should do a fdisk -l to see the physical devices. You will have to mount whichever drive you need to get the data from. mkdir /mnt/windows1 mount /dev/hda1 /mnt/windows1 -t ntfs -r Once you have the drive you need you can get your backup device setup. I used a [usb hard drive](http://linuxhelp.blogspot.com/2007/03/steps-to-manually-mount-usb-flash-drive.html)(beware some only turn on when active --MyBook) Now you can do a copy cp -r /mnt/windows1 /mnt/usbharddrive Voila you have your valuable data again.