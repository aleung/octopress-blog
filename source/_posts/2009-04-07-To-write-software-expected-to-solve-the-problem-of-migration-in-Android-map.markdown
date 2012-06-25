---
layout: post
title: "写了个软件，期望可解决Android中地图偏移的问题"
comments: true
date: 2009-04-07 08:08
tags:
- Android
- GPS_GIS
---
这两天用着G1的my location功能，发现真是很爽，即使不开GPS，单独依靠移动网络来定位在很多地方都已经相当准确，往往已经到了一个住宅小区的精度。但是地图模式的人为偏移就令人非常懊恼，在广州区域内偏差600多米，明明在天河公园东面它显示到西门那边。

Google地图加入的人为偏移是由于中国测绘法规的限制而加入的，这样造成my location定出来的位置在地图上偏差太远，已经失去使用价值了。而卫星图是没有偏差的，这就是为什么大家发现地图(map)模式误差大，卫星模式误差小的原因。  
  
在电脑上打开Google Map，对于一个标志性地方，在地图与卫星图上各标一个waypoint，比较两个坐标，就可以知道地图的偏差有多大。经过试验，发现在一个区域内（例如整个广州市），偏差基本上是固定的，而不同城市的偏差就不同了。  
  
利用这个特性，对一个区域的偏差进行测量校准后，整个区域的定位都能够根据这个校准值来校准。如果能够将各人在不同地方测得的校准值共享在网络上，这个手机的定位功能就会有实用价值了。  
  
今天先写了个程序实现了my location的校准显示，将广州市的校准值（初步测试发现对广东省范围都适用）内置在程序里了。后面我会继续改进这个程序，让校准值可以设置，期望最后能做到在网络上同步。

[![](http://photo1.bababian.com/upload15/20090407/9D46C0897B7336EFC463728A844EDBDF.jpg)](http://www.bababian.com/phoinfo/9D46C0897B7336EFC463728A844EDBDFDT)

程序的功能很简单，就是显示地图，并且将校准后的my location蓝点显示在地图上，也可以打开系统Map软件，打开后校准后的my location在屏幕正中心。

Update 2009-4-20：

软件改名为Here I'm!，并已建立专门的网站。下载和更新请到 [http://sites.google.com/site/hereimapp/](http://sites.google.com/site/hereimapp/)[](http://aleung.blogbus.com/logs/38092084.html)
