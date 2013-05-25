---
layout: post
title: "‘I Love’ audio player - basic design"
comments: true
date: 2006-08-29 02:19
updated: 2012-06-23 19:19
tags:
- SoftwareDev
- Idea
---
对于"I Love" audio player的想法, flyisland的批阅建议是请jobs同志执行。我也很希望jobs同志能够欣赏这个想法，更希望的是我也能有flyisland同志同样的运气，能得到一个赠送的iPod。在此之前，还是继续自我陶醉一下，下面是想到的初步设计：

![audioplayer](/attachments/2006/8/227012217_2b9c0d6ecd.jpg)

UI负责用户界面；Control接受UI的命令，进行播放控制；作品的选择、喜爱度的累积是profile的任务；媒体文件的播放是委托给player进行的。

![audioplayer.control](/attachments/2006/8/227012224_56936e3643.jpg)

JavaZoom.com的[jlGui](http://www.javazoom.net/jlgui/developerguide.html)是一个opensource的music player，它提供了一个很简洁实用的BasicPlayer API，用在这里正合适。

![basicplayer](/attachments/2006/8/227012248_2ce4e0e4e8.jpg)

UI，应该就是GUI了吧，有人会需要command line吗？甚至是外接LED显示屏？

![audioplayer.ui](/attachments/2006/8/227012242_fa610759ef.jpg)

Profile部分将是最重要也是最复杂的了，现在考虑得还不成熟，只有个大概。

![audioplayer.profile](/attachments/2006/8/227012232_3c57a934d1.jpg)