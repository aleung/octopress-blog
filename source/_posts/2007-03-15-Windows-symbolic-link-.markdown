---
layout: post
title: "Windows的symbolic link支持"
comments: true
date: 2007-03-15 21:32
tags:
- SysAdmin
- Windows
---
UNIX类的操作系统支持symbolic link [2]，可以通过"ln -s"命令来创建。但是在Windows操作系统中，却没有提供这样的功能，文件管理就不如UNIX方便。

虽然Windows的shortcut在功能上有点类似于symbolic link，但还不是一个层面的东西，因为OS并没有管理它，要靠应用程序来处理。在DOS窗口里面dir看看一个shortcut是什么样子的，就知道有什么不同了。

其实，windows在NTFS 3.0开始支持一种叫做 junction point 的特性[3]，可以看作为目录的symbolic link。可是微软总是喜欢把一些东西藏起来，比如这种junction point，你就无法直接通过windows自带工具来建立、管理。我是使用[xplorer2](http://www.zabkat.com/)这个explorer替代工具的时候，才发现有"Folder Junction"的功能(Edit - Paste Special - Folder junction)。在[1]中介绍了很多可以管理junction point的工具。不过，使用时还是要小心，它跟UNIX的symbolic link还是不完全相同，特别是删除的时候，junction point的行为有点怪异的[1][3]。据说Vista会提供与UNIX兼容的symbolic link支持。

利用junction point，可以在D盘建立一个目录，link到C盘来使用；也可以将分布在不同地方的多个子目录link到同一个目录底下，方便备份。

另外，Windows NTFS也支持hard link的[5]，但实际使用中symbolic link的作用更大。

Reference:

  1. Windows Symbolic and Hard Links: [http://www.shell-shocked.org/article.php?id=284](http://www.shell-shocked.org/article.php?id=284)
  2. Symbolic Link in Unix: [http://kb.iu.edu/data/aibc.html](http://kb.iu.edu/data/aibc.html)
  3. NTFS Junction Point: [http://en.wikipedia.org/wiki/NTFS_junction_point](http://en.wikipedia.org/wiki/NTFS_junction_point)
  4. Hard Link in Unix: [http://kb.iu.edu/data/aibc.html](http://kb.iu.edu/data/aibc.html)
  5. Hard Link in Windows: [http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/fsutil_hardlink.mspx?mfr=true](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/fsutil_hardlink.mspx?mfr=true)
