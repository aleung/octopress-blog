---
layout: post
title: "了解JVM的内存管理与垃圾回收"
comments: true
date: 2007-03-08 04:31
tags:
- SoftwareDev
---
Java语言具备GC(垃圾回收)的能力，内存管理不需要应用程序去过问，这很方便。但是，GC是怎么进行的，JVM的内存参数应该怎么调整，如何优化，往往我们不是太清楚。看过一些资料后，对Sun JVM的内存管理以及垃圾回收的机制大概有了一个概念，这里将这些资料归纳和翻译出来。本文内容主要基于Sun JVM 1.3.1，在后续版本中有不少优化措施，但是这些基本概念还是不变的。

这里假设大家对GC的概念和基本原理都已经了解，不详细叙述了。

当JVM进行GC的时候，是要消耗CPU资源和需要一定时间的，这会影响到程序的正常运行，因此需要尽可能减少GC消耗的时间。Java程序运行过程中，对象的生命周期有长有短，其中相当大部分是都是比较短命的，例如局部的对象一用完就可以回收了。在大多数情况下，只要能够及时回收这些短命对象的内存，就能够确保JVM有足够内存来分配给新的对象。因此JVM采用一种分代回收(_generational collection_) 的策略，用较高的频率对年轻的对象(young generation)进行扫描和回收，这种叫做minor collection，而对老对象(old generation)的检查回收频率要低很多，称为major collection。这样就不需要每次GC都将内存中所有对象都检查一遍。

Sun JVM 1.3 有两种最基本的内存收集方式：一种称为copying或scavenge，将所有仍然生存的对象搬到另外一块内存后，整块内存就可回收。这种方法有效率，但需要有一定的空闲内存，拷贝也有开销。这种方法用于minor collection。另外一种称为mark-compact，将活着的对象标记出来，然后搬迁到一起连成大块的内存，其他内存就可以回收了。这种方法不需要占用额外的空间，但速度相对慢一些。这种方法用于major collection.

在JVM 1.3及以后的版本中，还有其他可选的内存收集方法，通过特定的参数来设定。例如：增量式回收，每次只处理一小部分；替代单线程copying的多线程并行回收；替代mark-compact的concurrent mark-sweep回收等等。参考资料[4][5]中有更多描述。

JVM管理的内存，通常叫做堆(heap)，可以用下面的图来描述。

JVM启动后，保留一段地址空间，这个空间的大小由-Xmx指定。这块空间的大小就是heap可能的最大值，但一开始不一定全都分配了物理内存，初始分配的heap大小由-Xms指定，如果-Xms小于-Xmx，剩余部分是virtual的，当需要的时候，再向OS申请。

绿色部分是young generation的内存，由一块Eden(伊甸园，有意思)和两块Survivor Space(1.4文档中称为semi-space)构成。新创建的对象的内存都分配自eden。两块Survivor Space总有会一块是空闲的，用作copying collection的目标空间。Minor collection的过程就是将eden和在用survivor space中的活对象copy到空闲survivor space中。所谓survivor，也就是大部分对象在伊甸园出生后，根本活不过一次GC。对象在young generation里经历了一定次数的minor collection后，年纪大了，就会被移到old generation中，称为tenuring。(是否仅当survivor space不足的时候才会将老对象tenuring? 目前资料中没有找到描述)

浅蓝色部分是old generation的内存。

深蓝色部分称为_permanent generation_，是JVM用来保存class object和meta data，大小由-XX:PermSize和-XX:MaxPermSize指定。大量动态生成(编译)和加载class会增加这部分内存的耗用。

剩余内存空间不足会触发GC，如eden空间不够了就要进行minor collection，old generation空间不够要进行major collection，permanent generation空间不足会引发full GC。

很多参数会影响里面各部分空间的分配。-XX:MinHeapFreeRatio与-XX:MaxHeapFreeRatio设定空闲内存占总内存的比例范围，这两个参数会影响GC的频率和单次GC的耗时。-XX:NewRatio决定young与old generation的比例。Young generation空间越大，minor collection频率越低，但是old generation空间小了，又可能导致major collection频率增加。-XX:NewSize和-XX:MaxNewSize直接指定了young generation的缺省大小和最大大小。

[![fig4.gif](http://java.sun.com/docs/hotspot/gc/fig4.gif)](http://java.sun.com/docs/hotspot/gc/fig4.gif)

-Xmx  
set maximum Java heap size

-Xms  
set initial Java heap size

-XX:MinHeapFreeRatio=40  
Minimum percentage of heap free after GC to avoid expansion.

-XX:MaxHeapFreeRatio=70  
Maximum percentage of heap free after GC to avoid shrinking.

-XX:NewRatio=2  
Ratio of new/old generation sizes. [Sparc -client:8; x86 -server:8; x86 -client:12.]-client:8 (1.3.1+), x86:12]

-XX:NewSize=2.125m  
Default size of new generation (in bytes) [5.0 and newer: 64 bit VMs are scaled 30% larger; x86:1m; x86, 5.0 and older: 640k]

-XX:MaxNewSize=  
Maximum size of new generation (in bytes). Since 1.4, MaxNewSize is computed as a function of NewRatio.

-XX:SurvivorRatio=25  
Ratio of eden/survivor space size [Solaris amd64: 6; Sparc in 1.3.1: 25; other Solaris platforms in 5.0 and earlier: 32]

-XX:PermSize=  
Initial size of permanent generation

-XX:MaxPermSize=64m  
Size of the Permanent Generation. [5.0 and newer: 64 bit VMs are scaled 30% larger; 1.4 amd64: 96m; 1.3.1 -client: 32m.]

(上面给出的缺省值不一定准确，不同JVM版本和不同OS环境下会有不同)

这里给出的只是基本的介绍，下面reference中的文章都很不错，对进一步了解或者查找性能优化参数都有帮助。

Reference

  1. [http://blogs.sun.com/watt/resource/jvm-options-list.html](http://blogs.sun.com/watt/resource/jvm-options-list.html)
  2. [http://java.sun.com/docs/hotspot/gc/](http://java.sun.com/docs/hotspot/gc/)
  3. [http://java.sun.com/javase/technologies/hotspot/vmoptions.jsp](http://java.sun.com/javase/technologies/hotspot/vmoptions.jsp)
  4. [http://builder.zdnet.com.cn/2003/0415/85798.shtml](http://builder.zdnet.com.cn/2003/0415/85798.shtml)
  5. [http://java.sun.com/developer/technicalArticles/Programming/turbo/](http://java.sun.com/developer/technicalArticles/Programming/turbo/)
  6. [http://developers.sun.com/techtopics/mobility/midp/articles/garbagecollection2/](http://developers.sun.com/techtopics/mobility/midp/articles/garbagecollection2/)
