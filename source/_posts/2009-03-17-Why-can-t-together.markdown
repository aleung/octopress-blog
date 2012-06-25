---
layout: post
title: "为什么不能合在一起"
comments: true
date: 2009-03-17 08:20
tags:
- Idea
- TimeManagement
---
白鸦在他的[blog](http://uicom.net/blog/?p=818)上说，日历(calendar)和任务(task, todo)这两样东西为什么要分成两个独立功能，而不能合到一起？ 对此，我是有同感的。

在我的日常事务管理流程中，会将所有要做的事情都记录在todo list中，然后每天会做时间规划，根据todo list里面任务的优先级、due date决定当天需要做的事情，并且在日历中计划出处理这些事情的时间。当然，如果是会议请求，outlook就已经自动把它放在日历里面了。事情完成后，到todo list中将相应任务设置为completed。实际的执行过程中，时间安排可能会做出改变，我把日历里的项目调整为实际耗用的时间。这样，事务的管理、时间安排、时间耗用的跟踪都在todo list和日历这两个系统中完成了。

但是，分为两个系统，还是有麻烦之处：我必须同时查看todo list和日历，才能进行时间的安排；需要将todo list里面已经存在的任务手工填写到日历中。实际上，可以认为任务和日历中的事件(event)本质上是相同的东西：都是需要完成的一些事情。任务安排了具体时间之后就变成了日历中的事件。如果要将两者合并，属性上可以做这样一些设计：

统一后的属性|原Todo中的属性|原Calendar中的属性
------------|--------------|-----------------
Subject / Description | Subject / Description | Subject / Description
Due date | Due date |
Start date| Start date|
Complete status| Complete status|
Estimation| Estimation| 作为缺省的duration
Repeat| Repeat| Repeat
Start time / End time| Start time / End time|
Where| Where (不是所有todo软件都有此属性)| Where
Priority| Priority|

还有其他的一些属性就不一一列出了，但已经可见两者其实大部分都是重叠的。

两者合并后，在展现上，能够将一天里已经安排好具体时间的事件，和没有安排具体时间，但是需要在这天内做（根据Start date和Due date）的任务显示在同一界面上，什么时候该做什么就一目了然了。在操作上，将一个任务拖放到日历中，或者输入开始结束时间，就能为任务安排计划的执行时间，并且显示在日历中。
