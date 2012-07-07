---
layout: post
title: "解决Solaris中sendmail不断报错的方法"
comments: true
date: 2003-04-05 00:12
tags:
- SysAdmin
---
为解决sendmail不断报出unable to qualify domain name问题，修改/etc/hosts文件，在本机主机名后面加上一个点。但是引起了另外的问题，当程序调用gethostbyname()时失败。解决方法是在/etc/hosts为本机IP指定两个名字，一个仅仅是主机名，另一个是主机名加域名（在这里没有域名，只是加个点）。
