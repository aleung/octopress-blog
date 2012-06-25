---
layout: post
title: "Andriod TransProxy"
comments: true
date: 2010-09-28 06:13
tags:
- Anti-GFW
- Android
---
关注我之前写的Android翻墙系列文章[1](/2010/06/02/Consider-how-in-the-Android-mobile-phones-in-use-SSH-tunnel-over-the-wall), [2](/2010/06/07/Android-SSH-tunnel-2010-06-06-), [3](/2010/07/31/Android-SSH-tunnel-prototype)的朋友请留意一下这个应用：[TransProxy](http://forum.xda-developers.com/showthread.php?t=766569)。这个应用的v3版本集成了HTTP transparent proxy (使用[Transproxy](http://transproxy.sourceforge.net/))、HTTPS/SOCKS redirector (使用[redsocks](http://darkk.net.ru/redsocks/)) 、iptables script 并提供了配置、控制的界面。理论上，结合TransProxy和ConnectBot就可以实现Android上通过SSH tunnel翻墙，请参考[这里](/2010/06/02/Consider-how-in-the-Android-mobile-phones-in-use-SSH-tunnel-over-the-wall)的第二张图片：ConnectBot实现了SSH client，TransProxy提供了transparent socks proxy并且设置iptables。

不过，我刚刚将TransProxy和ConnectBot配合运行起来试了一下，并没有成功，并且手机又遇到了100% CPU占用的问题，只能手工将ConnectBot kill了。ConnectBot当前最新的snapshot版本都还是没有解决这个[bug](http://code.google.com/p/connectbot/issues/detail?id=299)，大家快去给这个bug加星，促使开发者优先解决吧。

鉴于我目前暂时处于肉身翻墙状态，没有积极性去进一步实验了，各位动手试试吧，有结果记得共享出来。另外，目前TransProxy的作者还在相当勤快的改进这个应用，不妨到xda-developers论坛与他交流。  
