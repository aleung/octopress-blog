---
layout: post
title: "在考虑怎样在Android手机上使用SSH tunnel翻墙"
comments: true
date: 2010-06-02 14:53
tags:
- Anti-GFW
- Android
---
目前在PC上使用SSH tunnel翻墙是一个相当广泛使用的方案，国外SSH帐号的获取不是太难，购买起来价格也比VPN帐号要便宜。但是在智能手机上目前还不能使用SSH方案，只能用VPN。Android系统是基于Linux的，特别是root了之后，基本上要玩什么都随心所欲了。因此Android上是应该也可以使用SSH tunnel的。

先看看Windows上是怎样设置SSH翻墙的：

  * 首先需要有个支持dynamic port forwarding的SSH client，这个SSH client能开放出SOCKS服务
  * 如果浏览器(或者其他程序)支持使用SOCKS proxy，设置使用SSH client开放出来的SOCKS端口，就可以使用了
  * 如果浏览器(或者其他程序)不支持SOCKS proxy，只支持HTTP proxy，就需要再运行一个支持SOCKS的代理程序（例如Privoxy），将HTTP转成SOCKS

![](http://farm5.static.flickr.com/4046/4662779602_743c1c50c2_b.jpg)

在Android上就要麻烦一点了，因为需要网络通讯的不仅仅是浏览器，很多Android application都需要访问网络，但是它们并不支持proxy----连HTTP proxy都不支持，更不用说SOCKS了。它们只会直接连接目标站点，如图中虚线所示，然后撞墙。

但是Linux有iptables，可以设置iptables规则（需要root权限），将HTTP/HTTPS请求forward到一个transparent SOCKS proxy去。transparent SOCKS proxy的作用是将任意TCP连接转成SOCKS连接，这样就可以走SSH tunnel出去了。我分析过几乎所有的Android应用访问网络都是走HTTP/HTTPS协议的，所有仅仅处理HTTP/HTTPS就够了。

![](http://farm5.static.flickr.com/4023/4662779594_32dd9773cb_b.jpg)

Android上支持dynamic port forwarding的SSH client有[ConnectBot](http://code.google.com/p/connectbot/)，不过ConnectBot是一个有界面的应用，似乎不能由其他程序来控制，这样就不好做到翻墙软件的傻瓜化了。应该还是弄一个命令行的ssh client更合适。

在网上搜索了一下，开源的transparent SOCKS proxy有几个：[transocks_ev](http://oss.tiggerswelt.net/transocks_ev/)，[TranSocks](http://transocks.sourceforge.net/) 和 [redsocks](http://darkk.net.ru/redsocks/)。从文档上看，似乎transocks_ev比较适合。

但是，我现在面临的困难在于我不熟悉如何交叉编译Android平台程序（C语言），了解了一下如何搭建Android交叉编译环境，似乎挺麻烦（对我而言）。而且要让普通Linux上可运行的sshd和transocks port到Android上，也许还有一些代码要修改。不知道有没有Android系统高手能够帮忙？
