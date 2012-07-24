---
layout: post
title: "Xperia U 刷机"
date: 2012-07-21 23:20
comments: true
tags:
- Android 
---
昨天中午拿到国行 [Sony Xperia U](http://www.sonymobile.com/global-en/products/phones/xperia-u/) (ST25i) 手机，开机试了一下，索尼(原来的索尼爱立信)在Android 2.3基础上的定制系统做得还可以，但大陆行货里好多功能都被阉割得不成样子。Google系列的任何东西都去掉了就不用说了，很奇怪的是居然系统里没有自带任何应用市场，国内的山寨市场也没有。可以想象一个不熟悉IT的用户买到手机后就会局限在预装的那几个应用，连升级都没有途径。比较一下SonyMobile英文与中文网站对这款手机的介绍，就会发现中文产品介绍缩水不少，好多卖点都没有了。

这样的智能手机简直就是残废，于是下班后就开始研究怎么刷机。先是在国内的一些论坛上搜索，但国内论坛真是很没有营养，里面的信息的价值很低。Android hacker的大本营在XDA，相关问题还是要到[XDA论坛](http://www.xda-developers.com/)找靠谱。

关于Xperia系列刷机需要了解的基础知识，这篇 [All that u need to know before u begin](http://forum.xda-developers.com/showthread.php?t=1526866) 讲得比较清楚。

无论要刷什么，前提条件是先将 bootloader 解锁。关于解锁的教程很多，SonyMobile网站上就有详细步骤，XDA上的 [Xperia S/P/U/Sola Bootloader Unlocking & Relocking](http://forum.xda-developers.com/showthread.php?t=1527159) 附带了所需工具的下载。还有一个国产软件，号称是一键自动解锁。

但是，就在第一步，将手机boot到fastboot模式，就花了我好几个小时。按教程，手机在关机状态按着音量+键的同时将USB连接上电脑，就会进入fastboot模式，并且亮起蓝灯。但是无论我怎么反复重试，手机都不会亮起蓝灯，电脑上弹出对话框提示安装驱动程序，但当我选择操作时它又说USB设备已经断开，显示这样的错误信息：s1boot fastboot device unplugged。网上搜索，有人说是驱动没有装好的原因，最后还是从XDA里找到了[解决方案](http://forum.xda-developers.com/showthread.php?t=1554632)。原来，Xperia手机在进入fastboot模式时，会检测电脑是否已经装上了正确的驱动程序，如果没有，就会退出fastboot模式。而电脑端刚刚提示用户安装驱动，就发现手机已经关闭了连接，就取消了驱动的安装。这种问题可以用来做经典反面案例了。XDA帖子里的解决方法还是要拼手快：预先打开设备管理器，一插上手机后，里面会出现s1boot这个设备，赶紧鼠标右键选择升级驱动，升级驱动的对话框打开后，即使设备断开了还是可以继续进行驱动程序的安装。终于看到传说中的蓝灯了，原来是在下面透明发光条发出很亮的蓝光，我之前还一直以为是右上角摄像头旁边的小灯。

接下来就非常顺利了，解锁bootloader后，用fastboot安装了ROM和Kernel。我都没有另外用其他工具了，感觉fastboot用起来就很简单，很多人不喜欢可能是因为命令行操作吧。关于fastboot使用，推荐看cyanogenmod的wiki里的[介绍](http://wiki.cyanogenmod.com/wiki/Fastboot)。

我刷的ROM是[KA02 Xperia SSpeed](http://forum.xda-developers.com/showthread.php?t=1684062)， 还是Android 2.3.7，Sony还没有为Xperia U 提供Android 4.0 ICS。Kernel是[Advanced Stock Kernel](http://forum.xda-developers.com/showthread.php?t=1688147)。刷完开机，见到熟悉Google账号绑定，然后Gmail、通信录、日历同步、Google Play都回来了。Root也有了，收工。
