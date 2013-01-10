---
layout: post
title: "怎样在OSX上adb连接小米2"
date: 2012-12-30 20:24
comments: true
tags: Android 
---
小米2手机在Mac OSX上，adb识别不到设备，Eclipse的DDMS也连不上设备，这样手机就不能用来开发了。实际上，只有在Windows上才需要安装USB驱动程序，在Linux、OSX上并不需要，设别不到小米2手机是因为adb不知道这手机的USB Vendor ID。Android SDK[文档](http://developer.android.com/tools/device.html)里就列出了一些Android设备厂商的vendor ID，不过当然不包括小米，因此要自己找出小米的vendor ID。用IORegistryExplorer（据说是包含在Developer Tool里，反正我的机器上装了）可以查看连接上的USB设备的信息。选择IOUSB，可以看到名为“MI 2”的设备，idVendor是0x2717。

将这个Vendor ID作为独立一行加进文件 ~/.android/adb_usb.ini 中。装了Android SDK这个目录和文件应该就存在的，如果不存在就自己创建。修改完adb_usb.ini后执行 adb kill-server 重启adb，再执行 adb devices，就能看到小米2手机了。再打开Eclipse，也能够正常设别到手机，一切OK。

{%img /attachments/2012/12/osx_usb.jpg %}
