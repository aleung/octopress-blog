---
layout: post
title: "Android用户的好消息：用ssh翻墙"
comments: true
date: 2011-02-11 20:41
updated: 2011-02-13 19:59
tags:
- Android
- Anti-GFW
---
之前我写过一系列[文章](http://good-good-study.appspot.com/blog/posts/90001)介绍Android系统用ssh翻墙的想法，现在有人把整合的软件做出来了：[sshtunnel](http://madeye.me/2011/02/10/ssh-tunnel-on-the-android-application-puff-android-edition/)

与TransProxy类似，sshtunnel也是内置了redsocks作为HTTPS/SOCKS redirector，使用iptables重定向网络流，不同的是它还使用了trilead-ssh2代码（作者说来自于ConnectBot）来做SSH client，因此这一个程序就可以完成整个过程，操作简单。根据文档说明和大致浏览代码，貌似没有使用SSH dynamic port forwarding，而是要求SSH server端安装了HTTP代理服务器，SSH连接纯粹是tunnel了到服务端HTTP proxy的连接。

Update: [作者说](https://twitter.com/#!/ofmax/status/36717058762739712)，不使用SSH dynamic port forwarding是因为受限于Android手机的机能。我之前测试用ConnectBot做SOCKS proxy的时候就总是遇到CPU占用100%的问题，估计作者说的就是这个。
