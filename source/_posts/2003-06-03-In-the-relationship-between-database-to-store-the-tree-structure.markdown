---
layout: post
title: "在关系数据库中保存树状结构"
comments: true
date: 2003-06-03 00:07
tags:
- SoftwareDev
- Database
---
看了 [Trees in SQL](http://searchdatabase.techtarget.com/tip/1,289483,sid13_gci537290,00.html) 这篇文章, 提出了一个巧妙的方法来在关系数据库中保存树状结构. 在记录中不是保存父子关系, 而是记录着树的遍历的编号, 通过利用编号可以在单个SQL语句中获取某个节点的所有子节点等操作.   
据介绍, 著名论坛jive在2.5版本起增加了catalog, 就是用这种方法实现树状关系帖子的保存.  
  
[http://searchdatabase.techtarget.com/tip/1,289483,sid13_gci537290,00.html](http://searchdatabase.techtarget.com/tip/1,289483,sid13_gci537290,00.html)  
  
_需要注册一个帐号才能进入上面的连接. 本来想直接帖出来, 可是转贴后格式会乱_
