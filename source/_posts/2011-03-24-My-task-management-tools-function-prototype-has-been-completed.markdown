---
layout: post
title: "我的任务管理工具功能原型已经完成"
comments: true
date: 2011-03-24 05:13
tags:
- Android
- SoftwareDev
- TimeManagement
---
在[上一篇blog](http://good-good-study.appspot.com/blog/posts/119001)里面列出了我个人对任务管理工具的需求以及对比较热门的工具的分析。由于现有工具没有那款能够满足我的几点主要需求，于是就产生了自己做一个的想法。前几天，功能性原型完成，已经正式用了起来，下一步要做的工作是界面设计和交互设计。

目前做的是Android手机应用，web端还没有考虑。一方面是手机几乎不离身，保证了随时可用，另一方面是web端使用现有的服务已经勉强可以满足要求，下面我会说明。

从功能定位上，主要是受了[StayUseful](http://stayuseful.com/)和[TeuxDeux](http://teuxdeux.com)的影响，目标是要做成简洁、轻量级的任务列表，强调对任务进行计划安排，专注于当天以及最近几天的任务。

  * 单日任务列表：在一个页面里列出一天的所有任务。只显示计划在此一天执行的任务，保证专注性。缺省的页面是当天，可快速导航到最近几天。
  * 用简单的操作可更改任务的执行日期。
  * 过去没有完成的任务自动放到"今天"，要么马上去完成它，要么重新安排执行日期。
  * 任务可设置due date，对于有due date的任务，会显示出剩余天数或者过期天数。
  * 任务不设优先级，但可以加星。单日任务列表中，任务可以拖放排序。
  * 数据与云端同步。

因为目标要做到数据与云端同步，这自己从零开始实现工作量太大了，所以一开始就计划基于网上现有的服务基础之上开发。最初物色的对象是Google Tasks，但发现Google Tasks尚未开放API，虽然已经有Android应用可以做到与Google Tasks同步，但它们是靠hack Google的网页前端与后端通信接口来实现的，实现起来既麻烦也不可靠。接下来发现Google Calendar是一个更好的选择：

  * Android Calendar与云端的Google Calendar之间几乎是实时同步，无论在哪边做修改，几秒钟之后另外一边就能看到变动。
  * Android Calendar提供Provider接口供其他Android应用调用，开发简便，并且Android Calendar是open source的，有问题可以直接看代码。
  * Web端可使用Google Calendar，虽然不能实现我需要的全部功能，但基本的都可以做到。
  * 为实现日程表跟任务列表的整合提供了可能。Google Calendar还能与其他系统同步，提高扩展性。

具体做起来其实不难，就是将一些task专有的数据放进Calendar的Event里面。Google Calendar的event数据对象没有供存放扩展数据的字段，我就将task数据用JSON格式放进description里面了。

好了，下一步怎样做出简洁、易用、高效的交互界面才是困难所在。另外，给它起个好名字也很难，有什么建议么？
