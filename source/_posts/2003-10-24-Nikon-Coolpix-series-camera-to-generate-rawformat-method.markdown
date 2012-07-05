---
layout: post
title: "使Nikon Coolpix系列相机生成raw格式文件的方法"
comments: true
date: 2003-10-24 02:53
tags:
- Misc
---
使Nikon Coolpix系列相机生成raw格式文件的方法：将机器的id字符串由"NIKON DIGITAL CAMERA"改为"DIAG RAW"。利用photopc软件，通过串口数据线可以发送命令给相机实现修改

    photopc id"DIAG RAW" query 
    photopc id "NIKON DIGITAL CAMERA" query

打开DIAG RAW模式后，每次拍摄会生成两个文件，一个是正常的jpg，另外一个是raw格式文件，在950上大概文件大小是2.35M左右。

photopc软件可以控制相机的操作，例如设置光圈快门、拍摄照片等。

RAW2NEF工具可以将raw格式文件转换为Nikon的NEF文件格式，这是nikon的公开raw格式，photoshop plugin可以打开。[http://e2500.narod.ru/raw2nef_e.htm](http://e2500.narod.ru/raw2nef_e.htm)

好像另一个dcraw软件也是做这工作的：[http://www.cybercom.net/~dcoffin/dcraw/](http://www.cybercom.net/~dcoffin/dcraw/)
