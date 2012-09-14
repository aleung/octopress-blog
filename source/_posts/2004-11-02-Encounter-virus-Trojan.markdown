---
layout: post
title: "遭遇病毒(木马)"
comments: true
date: 2004-11-02 22:45
tags:
- Software
- SysAdmin
---
操作系统刚重装才一天，就出现了莫名其妙的问题。svchost进程就会把CPU全占满，使得系统几乎没有响应。用procexp（Process Explorer）来查看进程，这个svchost.exe确实是系统自带的，是系统服务的宿主进程。

Reboot机器，重试几次，发现不连上网的时候一切正常，一旦网络连上了，CPU就被那个svchost占满，如果把那个进程杀死，系统就不能正常工作。当时刚刚安装了Norton AntiVirus，它的auto update服务会在网络连上的时候自动开始，所以我怀疑是NAV update服务引起的。把NAV卸载了，问题依然存在。因为那个svchost是RPC服务的，我又以为是操作系统的问题，什么文件的版本乱了，就用安装盘采用重装式修复，可是还是不正常。

毫无头绪之际，再度用procexp监视进程树。这次发现一些异常了：svchost启动了一些tftp进程。这些tftp在做什么？从219.137.217.181下载文件，下载的文件名为atiphexx.exe、winupdatez.exe等，还有不少没有记下来。很奇怪啊，是否系统中了木马？检查一下tftp，也是系统自己的东西，没有异常，那是谁启动这些进程的呢？

安装spybot，检查系统，没有查出木马程序。但是，过了一会，注册表监视器报警了，有进程要往注册表的系统启动run项目里写入新内容，居然就是winupdatez.exe。这不用说都是木马了。在winnt\system32目录下找到这个winupdatez.exe，删除。再检查注册表，把相关项目都删除。重启系统，正常了。

开始时太大意了，居然没有第一时间检查注册表的run里面有没有异常，白白浪费了两晚时间。不过，当初进程树里面看到的都是系统的进程，没发现这个winupdatez进程，所以才没有怀疑是病毒/木马。是否还有其他名称的木马存在呢？我心里还是没有底。不过系统现在运行正常了。新装系统，真的是要把所有防护措施都做好才能上网:(

这次用到工具软件Process Explorer很不错，能够看到很多Task Manager没有的进程信息，是[ www.sysinternals.com](http://www.sysinternals.com/) 的出品。Sysinternals还有好几个不错的小软件，都是监视系统运行情况的，包括tcpview、filemon、regmon等。

系统除了安装防火墙之外，还应该装上一个可以监视注册表改动的工具。现在修改注册表的网页太多了。我用的是Spybot，兼有木马查找和注册表防护的功能，我喜欢它能够从网上update规则。
