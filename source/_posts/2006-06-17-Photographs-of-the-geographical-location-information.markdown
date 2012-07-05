---
layout: post
title: "照片的地理位置信息"
comments: true
date: 2006-06-17 08:59
tags:
- GPS
- software
- ImageProcessing
---
在JPG图像的 EXIF meta data 中，有一组是与地理位置信息相关的，包括经纬度、高度、方向等，可以记录拍摄图像当时的位置信息。不过，目前内置GPS或者带有GPS接口的相机还非常少见，所以这组数据通常是空的。

[World-Wide Media eXchange](http://wwmx.org/) 提供了免费工具 WWMX Location Stamper，能够依照数码照片的拍摄时间将GPS记录的track的位置信息设置到照片的EXIF中。只要在拍摄的过程中同时打开GPS将track记录下来，通过这个软件处理，照片就能附加上坐标了。

照片添加了地理位置信息，日后的整理就会更加方便，不仅不会忘记照片是在什么地方拍的，而且以后一定会有软件能够根据坐标来管理照片，例如查找在某个区域拍摄的照片。

就目前而言，能够利用EXIF地理位置信息的软件还不多。[IExif](http://www.opanda.com/cn/iexif/index.html)是一个EXIF查看工具，对于带有地理信息的照片可以在GoogleMap上面定位出拍摄地点。最新的Google Picasa Beta也增加了对geotag的支持，包括export到Google Earth，或者在Google Earth上给照片标注坐标。[Google Picasa Web Album](http://picasaweb.google.com/) 开始公开测试了，相信Google一定会将旗下的GoogleMap，GoogleEarth，GooglePicasa集成在一起。

至于目前广泛使用的在线相册flickr，本身并不支持EXIF的地理信息，但是可以通过tag来记录照片的坐标，称为[geotag](http://en.wikipedia.org/wiki/Geotag)。有大量的第三方网站和插件能够处理或使用geotag，通过geotag来聚合信息。[Flickr Importer](http://www.flickr.com/groups/flickrimportr/) 可以在上传照片的时候根据EXIF自动创建geotag。也有少数第三方flickr服务能够直接使用EXIF地理信息，例如[FlickrFly](http://www.roblog.com/flickrfly-docs/)。

我的[flickr](http://www.flickr.com/photos/aleung/)里有一些照片已经标记上了，并且可以使用FlickrFly飞过去看看。
