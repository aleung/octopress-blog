---
layout: post
title: "用作OziExplorer底图的ASTER卫星遥感图的制作"
comments: true
date: 2003-12-04 18:04
tags:
- GPS_GIS
---
  1. 下载ASTER L1B数据，AST_09(或者AST_07,未知有何区别)格式。数据包括两个文件，hdf0是15m VNIR(band 1-3)，hdf1是30m SWIR(band 4-9)，一般只需要hdf0。 
  2. 用MultiSpecWin打开hdf文件，将各个band分别保存为tif文件 
  3. 用photoshop打开三个band的tif文件，按照RGB模式通道合成为一个文件，采用231合成的效果比较好。 
  4. 在photoshop中对图像进行直方图调整、锐化，保存为jpg 
  5. 用ECW compressser将jpg转换为ecw格式 
  6. 在OziExplorer中校准, 投影设为UTM，然后用META FILE里面的四个点来作为校准点，分别是图像（包括黑色部分）的四个角。  

