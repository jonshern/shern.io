---
title: 'Entity Framework and Sql Server Database Projects'
date: Thu, 26 Nov 2009 00:36:47 +0000
draft: false
tags: ['code']
---

Entity Framework 4 adds a bunch of new features this time around. One of which is to push model changes back to the database. If you are using one of the Database projects included in the Database Edition you may wonder how you are going to bridge the gap between the two projects. Unfortunately there is no good way to do this. Right now MS is prescribing to generate alter scripts from the EDMX model and add as a script to the database project.