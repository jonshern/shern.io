---
title: 'Creating File Paths in C#'
date: Fri, 08 Aug 2008 06:00:17 +0000
draft: false
tags: ['code']
---

Path.Combine is an easy and reliable way to combine path and file information.

**Before**

```
DataFile = Environment.GetFolderPath(Environment.SpecialFolder.Desktop) + "" + "samplefile.xml"
```

**After**

```
DataFile = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.Desktop),"samplefile.xml");

```

  
When using Special Folders you need to call Environment.GetFolderPath to return the actual path.