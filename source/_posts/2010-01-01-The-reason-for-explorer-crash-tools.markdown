---
layout: post
title: "查找explorer crash原因的工具"
comments: true
date: 2010-01-01 06:59
tags:
- Software
- SysAdmin
---
不知道装了什么软件之后，只要在文件管理器中点击鼠标右键，Windows Explorer就会崩溃，重新启动依然如是。

找到[Nir Sofer](http://www.nirsoft.net/)出品的小工具 ShellExView，这个工具分析注册表，列出所有注册到explorer的shell extension及相关信息，并能够enable/disable选定的extension。找到可疑的extension，将它disable，然后再进行一次右键操作，看看还会不会crash。如此排除法很快就定位出什么软件是罪魁祸首，将它卸载或者将extension disable，问题就解决了。

比较有用的信息是extension type，file extensions，file created time。首先关注文件创建时间，找出最近新增的extension，因为以前正常，最近才出问题，那引起问题通常是新装的软件。不过也不能一概而论，也有可能是以前安装的软件，因为最近的环境更改而出现了问题。另外可以根据extension type和file extensions进行过滤，例如我遇到的问题是右键菜单引发崩溃，那重点关注extension type为Context Menu的那些项。还有，在任何类型文件上按右键菜单都会引发崩溃，所以不大可能是由只针对特定文件类型的extension引发的。虽然系统注册的shell extension很多，例如我这个新装没多久的Vista系统就已经有三百多项，但是大部分都是Microsoft出品的，一般来说还是比较可靠的，只要关注非Microsoft的就可以了，找起问题来还是很快的。
