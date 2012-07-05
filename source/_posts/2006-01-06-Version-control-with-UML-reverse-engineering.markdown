---
layout: post
title: "Version control with UML reverse engineering"
comments: true
date: 2006-01-06 02:18
tags:
- SoftwareDev
- Idea
---
现在的版本管理工具都具有版本比较的功能，可以比较一个文件不同版本间的差别，将变化的代码行标记出来。对于常见的开发语言，也有工具可以进行UML逆向工程，根据代码生成出class diagram，这提供了一个概览代码的途径，对于理解代码整体结构很有帮助。有没有可能再进一步，将UML逆向工程与版本管理结合在一起呢？

我设想的工具具有这样的功能：将两个版本的project进行比较，生成出class diagram，并且将变化过的（增加/修改/删除）类、方法、关系等UML元素用特别颜色标记出来。这样，就可以方便的看到两个版本之间修改过什么类、方法，而不仅仅是只是在代码行的级别。

这样，做代码review也方便多了，知道重点看哪里。
