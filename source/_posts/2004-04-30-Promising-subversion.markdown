---
layout: post
title: "前景光明的subversion"
comments: true
date: 2004-04-30 23:47
tags:
- SoftwareDev
---
[Subversion](http://subversion.tigris.org/)这个以替代CVS为目标的版本管理工具，看来已经开始获得广泛支持了。经历了4年开发，大概1个月一次小版本发布，今年年初终于发布了version 1.0 release，开发过程平稳而谨慎。我去年曾经试用过一个interim release版本，作为本地个人使用的版本管理工具，已经感觉相当满意，基本可替代CVS。现在release版本发行后，更是在网上看到一些人说打算从CVS迁移到SVN了。在网上可以找到一些达到实用阶段的兼容subversion的软件工具、plugin，从另一个侧面也反映出subversion正在一步步迈向自己的目标。

以前公司的开发团队使用CVS，配合使用[CVStrac](http://www.cvstrac.org/)，对于这样的搭配我感到比较满意。CVStrac作为轻量级的issue tracking工具，确实做得很到位，issue/bug record、CVS track-in record和wiki的无缝整合，构成了简单而实用的web base软件项目管理系统，对于小型项目非常合适。

现在，与SVN配合的类似工具也有了，[Trac](http://www.edgewall.com/products/trac/)，完全就是CVStrac的一个clone，不过是用python开发的，从screenshot上看很漂亮，细节上似乎做得比CVStrac要好。不过现在公司用ClearCase，没有机会试用这些轻量级工具了。我觉得ClearCase的client并不好用，缺乏象cvsweb这样的直观的文件、目录浏览、trac这样的check-in浏览功能。
