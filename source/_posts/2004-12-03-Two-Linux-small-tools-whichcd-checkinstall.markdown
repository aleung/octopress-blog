---
layout: post
title: "两个linux小工具：whichcd, checkinstall"
comments: true
date: 2004-12-03 19:02
tags:
- SysAdmin
---
现在的linux安装盘通常都有几张，要找一个rpm包在哪张盘就很麻烦了，要一张张放进光驱里面看。在sourceforge.net上面找到whichcd这个小工具，能够告诉你某个rpm在第几张光盘，也能告诉你当需要某个文件/命令时应该安装哪个rpm。

估计它是预先将主要linux发行版的包的资料都保存起来了，支持的linux发行版本有限，RedHat的最齐。

另外一个工具叫做CheckInstall，通过记录软件make install过程中新建和拷贝了什么文件目录，自动生成rpm。（以前有个工具叫做installwatch，现在已经被包含到CheckInstall软件包里面了）。这就大大简化了rpm的制作过程，一个软件只有源码和makefile的，也能够直接制作出rpm，不需要写spec了。当然，这工具只能生成简单的rpm，不能做到检测依赖性、运行pre/post script。不过，大部分情况下，能做到这样也就够了。
