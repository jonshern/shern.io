---
title: 'Cascading Dropdown Enable Override'
date: Sat, 03 Feb 2007 06:44:16 +0000
draft: false
tags: ['code']
---

When using the Cascading Dropdown in the Asp.net Ajax toolkit I noticed a problem with disabling the dropdown. When you disable a dropdown that has an Cascading Dropdown extender wired up to it, it overwrites the setting and the dropdown ends up enabled. Disabling the extender prevents it from getting data. So in the current form, there is no way to disable the dropdown, and still retrieve data using the cascading dropdown control.