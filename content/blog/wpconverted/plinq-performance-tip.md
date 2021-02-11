---
title: 'PLinq Performance Tip'
date: Thu, 19 Nov 2009 19:33:18 +0000
draft: false
tags: ['code']
---

When using PLinq Memory Allocations can be the bottleneck. Anonomous types/reference types are allocated on the heap and will have to be garbage collected. The garbage collector was designed with low latency/fast UI thread design considerations. Try to use value types. Or in the app.config add <[gcserver](http://msdn.microsoft.com/en-us/library/ms229357.aspx) enabled="true"/> This enables server mode garbage collection. Typically workstation mode gc is the fastest, but when we deal with parellism and garbage collection across multiple processors server garbage collection can reap significant performance improvements.