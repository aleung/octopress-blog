---
layout: post
title: "一些与GPS相关的软件项目"
comments: true
date: 2003-08-27 01:11
tags:
- GPS_GIS
---
想写个GPS有关的软件，网上找到一些相关的项目: 

  * Java framework for GPS [http://www.aasted.org/gps/](http://www.aasted.org/gps/)  
这个项目的目标是为各种应用程序加入GPS功能提供framework. 目前提供的是与Garmin GPS通讯的功能. 
  * Chaeron GPS library [http://www.chaeron.com/gps.html](http://www.chaeron.com/gps.html)  
类似的项目,也是作为支持库，但功能更强/完善. 支持多种平台, 包括多种PDA. 而且这看来是一个active的项目, 由一个公司支持. 
  * JavaGPS [http://javagps.sourceforge.net/](http://javagps.sourceforge.net/)  
这个项目的范围似乎比前两个窄一些, 局限于从GPS中取得当前位置数据. 从代码看来，设计也不如前两个项目规范。

但是，这些项目看来都是只支持从GPS取数据，而不支持传送数据给GPS。

  * EPS [http://eps.sourceforge.net/](http://eps.sourceforge.net/)  
这是一个小应用程序，能够下载GPS数据并显示。这个程序能与Garmin GPS通讯，有上传/下载功能, 不过代码风格很差。
