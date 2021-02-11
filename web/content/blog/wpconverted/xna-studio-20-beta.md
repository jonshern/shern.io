---
title: 'Xna Studio 2.0 beta'
date: Wed, 21 Nov 2007 05:57:01 +0000
draft: false
tags: ['code']
---

The [XNA Studio 2.0 beta](http://creators.xna.com/beta/betahome.aspx) was released today. The biggest feature is the ability to create networked games. You can create them from Windows to Windows on your own own subnet right out of the box. You can create them from windows to Xbox 360 with a creators club membership. The functionality lives in Microsoft.XNA.Framework.Net namespace. (Great another Net namespace in .Net) It is great that it works out of the box with XBox Live, but at the same time it sucks that in order to play a game developed in XNA with a friend outside of my subnet I have to convince them to pay $100 to join the creators club. I can barely convince people to play games with me for free, how on earth will I convince them to pay $100. My solution create a proxy using wcf. Sure would be nice if MS used Inversion of control and a factory for their connectivity piece. Then it would be easy to just drop in a proxy. Looks like I have my thanksgiving project:)