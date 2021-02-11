---
title: 'Inversion of Control'
date: Wed, 12 Dec 2007 04:40:24 +0000
draft: false
tags: ['code']
---

Even though topics on Inversion of Control have been done to death. I want to revisit the subject to talk about the use of interfaces in inversion of control. I think Interfaces should be used to isolate all dependencies on the outside world, not just databases. When doing something with WCF. The client side service calls should also be using IOC so they can be isolated and the client side as a unit can be tested. ![Inversion of Control](http://www.jonshern.com/Images/InversionOfControl.gif)