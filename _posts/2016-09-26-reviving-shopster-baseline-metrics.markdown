---
title: "Reviving Shopster - Baseline Metrics"
date: 2016-09-26T13:25:42-03:00
---

Before any change to the codebase (apart for making it build), I figure it's a good idea to have a baseline of the amount of code the app has. For that, I'll use [`cloc`](http://cloc.sourceforge.net). Here's the initial output:

```
     127 text files.
     127 unique files.                                          
      25 files ignored.

github.com/AlDanial/cloc v 1.70  T=0.74 s (137.4 files/s, 8946.6 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Objective C                     47           1200            493           3688
C/C++ Header                    53            296            372            476
JSON                             2              0              0            117
-------------------------------------------------------------------------------
SUM:                           102           1496            865           4281
-------------------------------------------------------------------------------
```

This was run only on the folder where our code resides, so it's excluding all external dependencies (basically, the `Pods` folder).
