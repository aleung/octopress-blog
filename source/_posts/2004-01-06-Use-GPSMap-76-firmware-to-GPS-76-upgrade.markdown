---
layout: post
title: "用GPSMap 76 firmware给GPS 76升级"
comments: true
date: 2004-01-06 22:49
tags:
- GPS_GIS
---
根据网上新闻组有关“用legend firmware update venture”讨论的提示，今天冒险进行试验，成功的用GPSMap 76 v3.3 firmware将我的GPS 76升级。

升级的好处：

  * Number of waypoints from 500 to 1000
  * Number of track points in active tracklog from 2000 to 10000
  * Number of Routes / number of waypoints per route from 50/50 to 50/125

升级的坏处：

  * Not support POI database anymore
  * Warranty void

Steps:

  1. Download a firmware update for GPSMap 76 from Garmin website or from the following URL:   
[http://www.tramsoft.ch/gps/garmin_gpsmap76-firmware-upgrades_en.html](http://www.tramsoft.ch/gps/garmin_gpsmap76-firmware-upgrades_en.html)
  2. Extract the downloaded self-extractarchive file to a folder. There should be three files: a ROM image (rgn file), an update tool (exe file) and a readme file. 
  3. Notice that the ROM image is named as 017702000xxx.rgn,xxx is the firmware version number, 177 is the product number of GPSMap 76. You can't direct use this file to update the GPS 76 because of the incorrect product number. Change the rgn file name from 177 to 173 which is the product number of GPS 76. 
  4. Backup all date in your GPS 76. 
  5. The exciting time comes. Do it as a normal firmware update. 
  6. After about 2 minutes nervous waiting, it done. Enjoy your GPS 76 with 1000 waypoints and 10000track points!
