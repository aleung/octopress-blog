---
layout: post
title: "Line break in MANIFEST class-path"
comments: true
date: 2008-06-18 08:00
tags:
- SoftwareDev
---
1. NO space (at all) after the last jar name of each line; and  


2. TWO spaces on the next line.  
This will work:

Class-path: lib/1.jar  
lib/2.jar

Anything different from that will cause an exception or some of your jars will not be found on the classpath. 

From: [http://www.jroller.com/sgm/entry/manifest_class_path_too_long](http://www.jroller.com/sgm/entry/manifest_class_path_too_long)
