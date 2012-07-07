---
layout: post
title: "Swing components refresh"
comments: true
date: 2003-08-19 19:30
tags:
- SoftwareDev
---
Java swing编程，更改了一个JPanel的内部components后，需要改变一下窗口大小，新内容才能显示出来。  
解决方法：调用panel.validate()。  
对于container，添加或者改变了对象后，需要调用validate()才能重新布局并显示。
