---
layout: post
title: "How does Google Map My Location work?"
comments: true
date: 2008-05-11 08:22
tags:
- GPS_GIS
---
在今年初推出的[Google Map for Mobile](http://www.google.com/mobile/gmm/index.html)新版本中提供了称为My Location的功能，不需要GPS的支持，也能在地图上显示出当前手机所在的位置。这个功能着实让人眼前一亮。虽然移动网络本身就可以提供定位的能力，但是Google的My Location并不依赖于移动网络定位能力，完全绕开了运营商的控制。

My Location是怎样实现的呢？Google网站上介绍了大致的[原理](http://www.google.com/mobile/gmm/mylocation/index.html)：手机接收到移动基站的信号，根据基站的信息可以知道当前处于哪个基站的信号覆盖范围，如果基站的位置是已知的，就可以确定当前的大概位置了。

具体点说，这是一种叫做Cell-Id Positioning Method的技术。GSM网络（3G网络也一样）覆盖由Location Area组成，（Location Area是指mobile terminal可以任意移动而不需要进行location update的区域）。Location Area由LAI（Location Area Id）来标识，LAI由MCC，MNC，LAC组成。其中MCC是3位的Mobile Country Code，中国为460；MNC是2位Mobile Network Code，在国家内分配，中国移动为00；LAC为Location Area Code，在network内分配；可见LAI是全世界唯一的。在一个location area中设置一个或多个基站，基站天线的信号覆盖一定的区域，称为cell（小区）；根据天线的不同，每个基站可能包含1个或多个cell，定向天线的信号覆盖一个扇形范围，多个天线的扇区为不同的cell。每个cell有自己的Cell-Id，结合LAI和Cell-Id，就可以在全球范围内唯一确定一个cell。要进行定位，需要有一个cell坐标数据库，根据cell-id来查找位置信息。

Google的My Location表示格式为myl:MCC:MNC:LAC:CI。在Google Map for Mobile的about信息末尾可以看到。如果显示为myl:n/a，那就是手机不能提供cell-id信息。很不幸，我正在用的SonyEricsson w810c就不支持:(

下一个问题就是cell坐标数据是怎么来的？移动网络运营商提供，不是太现实。Google不可能与全球所有的运营商达成协议。特别是在中国，凡是涉及精确地理坐标的数据都属于机密范畴，然而我们发现My Location在广州是可用的，估计其他城市也可以。于是我猜Google是自己采集这些信息的，例如开辆车周围转，记录各处的cell-id。但有点奇怪的是，在广州我们发现在天河软件园得到的位置比较准确，但是在其他地方的就误差很大，并且表示精确度范围的圆圈半径非常大，将半个天河区都包含在内。难道Google只在天河软件园采集了数据，而其他地方没有？

最近，一个同事开发了一个程序，利用手机自带的GPS定位，将cell-id与坐标记录下来。我们讨论的时候才恍然大悟，Google很可能就是这样做的！有些Google Map for Mobile用户的手机是支持GPS的，当这些用户运行Google Map时打开了GPS，坐标数据以及cell-id就会发送到Google的服务器，等于有许多用户在替Google采集数据。在天河软件园的定位准确，就因为我们（当然也可能有其他人）在这里试验Google Map时打开过GPS。在我们这里，支持GPS的手机的拥有率不高，同时又使用Google Map的更少，因此Google掌握到的数据很有限。在没有采集到数据的地方，定位就不准确了。

为了求证，到Google网站上查看，在GoogleMobile的Privacy Policy中找到这样一段话：_If you use location-based products and services, such as Google Maps for mobile, you may be sending us location information. This information may reveal your actual location, such as GPS data, or it may not, such as when you submit a partial address to look at a map of the area_. 在help中又找到一段话：_Google takes geo-contextual information [from anonymous GPS-readings, etc] and associates this information with the cell at that location to develop a database of cell locations_. 看来我们的猜想是成立的。其实还可以通过做实验来证明：找一台支持GPS的手机，到一个定位有非常大误差的地方，在Google Map中enable GPS。过一段时间后，例如几天，因为Google处理数据可能有延迟，在关闭GPS的状态下看My Location，如果定位变得准确了就是Google使用了用户手机GPS采集的数据。不过我没有GPS手机，无法做这个实验。

在网上查找资料的过程中发现，其实Cell-id定位并不是一项新技术，不过它与Google Map的海量高清晰度地图结合起来，给业界带来了震撼。在Treo智能手机上，2006年就有国内的爱好者开发了[手机定位软件](http://www.palm119.net/blogview.asp?logID=42)，根据cell-id查询到当前位置，不过位置信息是地名，而不是经纬度坐标；Cell-Id对照地名信息的数据库是由用户补充完善的。在网上搜索一下，也可以找到好几个开放的cell-id坐标数据库，例如[OpenCellID](http://www.opencellid.org/)，只不过几乎都没有中国大陆的数据。Flickr为了配合ZoneTag（用手机拍照时记录下cell-id，从而可以知道照片拍摄的位置），还提供了[Cell Location API](http://developer.yahoo.com/yrb/zonetag/locatecell.html)。
