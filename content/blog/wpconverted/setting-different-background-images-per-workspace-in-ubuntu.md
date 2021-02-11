---
title: 'Setting Different Background Images Per Workspace in Ubuntu - 16.10'
date: Sat, 25 Mar 2017 21:24:01 +0000
draft: false
tags: ['ubuntu', 'Uncategorized']
---

Lately i have been doing almost all of my Development at home using Ubuntu. **Things I love:**

1.  Muliple Workspaces (I can get this in windows, but you can't do multiple backgrounds)
2.  Visual Studio Code (I love the fact that I can use on any platform)
3.  A good bash shell
4.  I have been doing a lot of stuff with AWS, and for the most part Linux is the ami of choice, so i just needed to get use to it.

Here is what i did to improve Number 1. In order to be able to differentiate the workspaces it is nice to have a different desktop image per workspaces **I am using Ubuntu 16.10.** Here is the documentation i started with http://askubuntu.com/questions/75998/is-it-possible-to-have-a-different-background-for-each-workspace There seemed to be a bunch wrong, for example the i could not find the compiz-fusion-plugins-extra I found that the compiz-plugins package must have changed name. Here is what i ended up with Go into the terminal \[code\] sudo apt install compiz-plugins compizconfig-settings-manager \[/code\] Opened up the Compiz Config Manager, and navigated to wallpaper. ![](https://jonsherndotcom.files.wordpress.com/2017/03/screenshot-from-2017-03-25-16-06-26.png?w=840) Make sure to Enable the plugin "Enable Wallpaper. Click on add, and add the four backgrounds. ![](https://jonsherndotcom.files.wordpress.com/2017/03/screenshot-from-2017-03-25-16-06-35.png?w=840) I then searched for Image Loading and enabled jpeg and png. ![](https://jonsherndotcom.files.wordpress.com/2017/03/screenshot-from-2017-03-25-16-07-06.png?w=840) I rebooted and voila i had 4 different backgrounds. It was pretty straightforward once i navigated all of the different versions of documentation. ![](https://jonsherndotcom.files.wordpress.com/2017/03/screenshot-from-2017-03-25-16-01-01.png?w=840)