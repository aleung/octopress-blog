---
layout: post
title: "Android手机上使用SSH tunnel：prototype"
comments: true
date: 2010-07-31 00:17
updated: 2010-08-23 10:04
tags:
- Anti-GFW
- Android
---
在上次写下[进展](/2010/06/07/Android-SSH-tunnel-2010-06-06-)之后，就真的没有任何进展了。本来打算完成之后会发布成开源软件的，但现在看来遥遥无期。有人问我iptables命令怎么写，就把简要介绍放这里吧。

  1. 首先确认你有Android系统的root权限，下面的操作要以root身份登录到shell进行
  2. 将transocks_ev放到Android系统中，目录随你喜欢。这里[下载](http://cid-e1c6bc07e3f505a3.office.live.com/self.aspx/.Public/transocks%5E_ev-android.zip)已编译的transocks_ev for Android可执行文件，解压后文件的MD5：6a0c3500bcd24ab7b706c8caf34f15ba
  3. 从Android Market下载ConnectBot，配置好你的ssh帐号，记得设置port forwarding，端口为9050
  4. 在ConnectBot中连接ssh
  5. 在shell中运行transocks_ev:  

	transocks_ev -S 127.0.0.1 -s 9050

  6. 在shell中执行iptables命令： 
    
	iptables -t nat -F 
    iptables -t nat -A OUTPUT -m owner --cmd-owner transocks_ev -j RETURN 
    iptables -t nat -A OUTPUT -m owner --cmd-owner org.connectbot -j RETURN 
    iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDIRECT --to-ports 1211 
    iptables -t nat -A OUTPUT -p tcp --dport 443 -j REDIRECT --to-ports 1211

  7. 这时所有的HTTP/HTTPS流量都会通过ssh tunnel了，但DNS查询不会，所以依然无法访问被DNS污染的网站
  8. 断开ConnectBot连接
  9. 在shell中kill掉transocks_ev进程
  10. 在shell中执行iptables命令：

	iptables -t nat -F

