---
layout: post
title: "用手机遥控Linux后台播放豆瓣电台"
comments: true
date: 2012-03-10 00:35
updated: 2012-03-11 15:06
tags:
- SoftwareDev
---
最近买了一个[EeeBox](http://en.wikipedia.org/wiki/Asus_EeeBox_PC)来做home server，装Ubuntu操作系统，有不少东西可以玩。

其中一个用途是连接到音响播放音乐。我比较喜欢听豆瓣电台，于是找一个豆瓣电台的客户端。Home server平常不接显示器，也不启动图形界面，常见的浏览器客户端、桌面客户端都不能用，要找命令行的。在github上找到Johnny Huang开发的[fmd](https://github.com/hzqtc/fmd)（Douban FM daemon），启动后在后台运行，正合我意。Fork下来给它增加了个安装脚本，能够安装为服务，开机就自动运行了。但fmd只是个后台进程，只有telnet命令接口，没法对它进行直接控制的，作者另外做了个命令行工具，叫做fmc（FMD client），用来控制fmd。但命令行工具在我这里也还是不能用----没有显示器没有键盘----必须能遥控才行。于是我在fmc基础上给它增加了一个web UI，称为[fmweb](https://github.com/aleung/fmweb)，通过手机浏览器访问，就可以控制播放了。于是，我现在在家里任何地方，都可以控制我的豆瓣电台的播放，不喜欢听，跳下一首歌。

这是在Android浏览器上的截图，在iPhone、iPad上也可以用。

![](https://lh6.googleusercontent.com/-ishGby7lpTk/T1ove7Z0-rI/AAAAAAAAAoU/94_iFdmfSKw/s400/Screenshot_2012-03-09-23-41-13.png)![](https://lh5.googleusercontent.com/-zXp_XdbbgNk/T1ovfpnkxbI/AAAAAAAAAoc/CN-7dV4U4Xg/s400/Screenshot_2012-03-09-23-55-39.png)
