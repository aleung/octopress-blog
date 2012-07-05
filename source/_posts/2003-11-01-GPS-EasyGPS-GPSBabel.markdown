---
layout: post
title: "GPS数据管理软件EasyGPS和格式转换工具GPSBabel"
comments: true
date: 2003-11-01 01:17
tags:
- GPS_GIS
---
EasyGPS是很好用的GPS数据管理器，能管理waypoint、route资料，它支持的文件格式是文件格式是gpx/loc。我使用过程中感觉它有以下优点：   
  
※ GPS waypoint的名称、说明字段的文字个数有限制，通常只能输入缩写。waypoint多了之后缩写很难理解，管理十分困难。在EasyGPS中，可以输入额外的信息，如中文说明、类别信息等，利用资料的整理。   
※ 双击选定一个点作为active point，程序会计算出所有其他点与active point的距离、方位角。如果用距离来排序，就能够快速找出某距离范围以内的waypoint。这是我认为这个软件最好用的功能。   
※ 软件提供检索功能，能够根据多种条件查找、选定符合条件的waypoint   
※ 可以打印waypoint清单。GPS中保存的waypoint没有详细说明，只要在出发前先打印一张单，名字具体对应的是什么地点就一清二楚了。英文机也不用愁啦。   
※ 从GPS上传、下载waypoint也是十分方便的   
  
我以前是用ozi文件分地区存放waypoint的，管理起来非常麻烦，先按大的地区分目录，再按小地区建立文件。而且在GPS多次导入导出，都分不清那些是新增那些是旧，有些还重复了。   
现在用了EasyGPS不用这么麻烦了，把所有点都放一个gpx文件里面，需要的时候利用搜索很容易就能找到，只要找到一个，按范围排序马上就把附近整个区域的所有点都找出来了，然后另存文件或者直接传到GPS。   
  
因此强烈推荐大家使用这种格式交换waypoint! :)   
  
EasyGPS是免费软件，在[http://www.easygps.com/](http://www.easygps.com/) 可下载；另外有ExpertGPS，功能更加多些，对route、track的管理更全面，不过是要钱买的。   
  
另一个推荐的是GPSBabel软件，可以在gpx / ozi / mapsource / mapsend 等几十种文件格式之间转换，除了格式转换之外，还有过滤waypoint的功能，例如可以将重名的、距离很接近的waypoint过滤掉，或者是只输出某个座标范围内的waypoint。   
这个是命令行工具，但同时也提供了GUI界面，方便使用。（GUI界面只能做格式转换，不能过滤）   
GPSBabel是open source软件，在[http://gpsbabel.sourceforge.net](http://gpsbabel.sourceforge.net/) 可下载。 

这里是ExpertGPS的[screen shot](http://www.lvye.org/uploadfiles/597018-easygps_introduce.gif)。
