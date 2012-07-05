---
layout: post
title: "Memory allocator影响多线程程序效率"
comments: true
date: 2005-11-10 02:14
tags:
- SoftwareDev
---
在8个双内核CPU的SUN 4900上运行多线程程序居然比起2个CPU的440更慢，发现原因在于memory allocator的问题，缺省的malloc库只适合于单线程，内存操作是序列化进行的。更换了mtmalloc就正常了。另外还有一个选择是用libumem。

[http://developers.sun.com/solaris/articles/multiproc/multiproc.html](http://developers.sun.com/solaris/articles/multiproc/multiproc.html)

[http://forum.sun.com/thread.jspa?threadID=24312&messageID=91895](http://forum.sun.com/thread.jspa?threadID=24312&messageID=91895)

换别的memory allocator也很简单，程序都不需要重新编译，仅仅修改启动脚本就可以了：

    LD_PRELOAD="/usr/lib/libmtmalloc.so" ./_my_application_
