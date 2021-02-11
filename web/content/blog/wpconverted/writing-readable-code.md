---
title: 'Writing Readable Code'
date: Thu, 20 Dec 2007 05:30:22 +0000
draft: false
tags: ['code']
---

I think the single most important principle to writing readable code(imho)is [SRP(Single Responsibility Principle)](http://en.wikipedia.org/wiki/Single_responsibility_principle) **In a nutshell** It is just making your classes and methods do one thing and that's it. Or at the very least name your methods accordingly if they do a couple of things. An MS Example of breaking the SRP Rule. bool Int32.TryParse(string s, out int result) **The benefits are numerous.** It is easy for other programmers to read your code. Your methods get much shorter. Refactoring becomes obvious when code starts to do more than one thing. Unit Testing is easier and clearer.