---
layout: post
title: "Apple Aperture 数码照片处理几板斧"
comments: true
date: 2011-09-27 22:04
tags:
- ImageProcessing
---
![](https://lh5.googleusercontent.com/-MGOVmxQlebs/ToHSOBbvsCI/AAAAAAAAAaQ/ZgUEHwlGelk/s800/aperture3.jpg)

以前都用Photoshop来处理照片，刚开始用Aperture，还真不太适应，用了几天兼阅读了help，慢慢有些了解了。对于照片的后期处理，其实用什么工具都大同小异，基本上就是几板斧。没拍好的照片也没有必要费心去调，直接删除就得了。

##  基本原则：使用RAW格式拍摄

RAW记录了拍摄时的原始信息，而jpg是已经经过相机处理之后的图像。用RAW格式提供了后期处理的更多可能性，例如曝光的增减（当然是小范围内了），白平衡的重新设定。

##  基本调整

  * **图像倾斜矫正及裁剪**：Viewer下方的工具栏有图标，或者用快捷键G和C。
  * **色温校准**：相机的自动百平衡经常不太准确，需要在White Balance设置正确色温，常见光源的色温表在[这儿](http://good-good-study.appspot.com/blog/posts/152001)；另外用 Enhance － Tint 可以分别调节高光／中间调／暗部的偏色。
  * **曝光调整**：用快捷键Alt－Shift－H打开暗部与高光溢出显示，调节Exposure让图像曝光正常。Highlights & Shadows可以挽救部分高光、暗部细节。如果要进一步调整，使用Enhance及Level。调整过程中，快捷键M可查看原始图像（Master）作对比。

调整方法见[RAW照片后期处理的常用操作](http://good-good-study.appspot.com/blog/posts/153002)

##  其他调整

  * **色差纠正**（消除紫边）：Chromatic Aberration
  * **降噪**：Noise Reduction
  * **图像锐化**：Edge Sharpen

##  常用快捷键

Key | Function
-----|--------------
V | Switch view mode (browser / split view / viewer)
I, H | Show Inspector
F | Full screen
W | Switch Inspector tab (Library / Metadata / Adjustments)
Y | Show metadata
T | Show metadata tooltips
M | Show master image
Z | Zoom 100%
` | Loupe
Alt-Shift-H | Highlight hot & cold areas
Alt-F | Show focus points
0~5 | Rating
