---
layout: post
title: "实现了我的 I Love Player 了"
comments: true
date: 2007-06-28 02:17
tags:
- Idea
---
去年写下关于 ["I Love" audio player](/2006/08/23/I-Love-Audio-Player/)的想法，曾想过自己开发的，但我没有开发这方面软件的经验，要学习的东西太多了，也没有时间精力来做。前段时间xlla朋友给我留言，介绍iPod的智能播放列表，我一下子兴趣又上来了，上网再次搜索一番，这次真给我找到了好几款支持智能播放列表的播放器软件，有些功能跟我期望的已经差不多了。我以前就想，不可能没有人想到的:)

第一个是KDE环境下的Amarok，特点是支持智能评分，能根据收听的行为，例如skip到下首歌曲时已播放的百分比，来给歌曲评定分数，而且评分方式是脚本定制的。这跟我期望的功能1（歌曲的喜爱度必须是在播放过程中日积月累的更新）一样了。可是我没有Linux环境，无法试用这个软件。Amarok下个版本计划支持Windows，只能等待了。

第二个是[musikCube](http://www.musikcube.com)，特点是使用嵌入式数据库SQLite来保存歌曲library，包括播放的统计数据，并且通过自定义的SQL查询where子句来生产动态列表，例如最近加入library的50首歌，听得最多的100首，最近一个月没有听过的歌等等，非常灵活。当然，完全不懂SQL语言的人就体会不到它的好处了。

musikCube是Visual C++开发的，open source。我把它的源代码下载来看，觉得代码挺好理解(比foobar2000好很多)，做点小修改不难。于是我写了一个[patch](http://aleung.blogbus.com/files/11829395480.zip) (基于musikCube 1.0 final)，实现了智能评分的功能。musikCube的歌曲数据库没有score字段，我不想修改数据库结构，就利用了它的times played字段当成score。每当播放下一首歌的时候，检查前一首歌播放了多少，如果播放了90%以上，score +5，如果少于10%，score -2，少于30%，score -1。总是被skip的歌得分就会很低，利用动态列表挑选出分数高的歌曲，就是我喜欢的歌了。

虽然musikCube不支持profile，但是支持多个library，可以建立多个library，里面是同样的歌曲，但播放统计却是独立的，这样也能满足我的需求2 (在不同环境、不同心情下喜欢的音乐是不一样的，有了profile功能就可以独立累计喜爱度)了。

虽然用起来还是没有我设想中那样简单，但已经大致符合我的期望了。等我使用一段时间再告诉大家效果如何 :)  


[Patch下载](http://aleung.blogbus.com/files/11829395480.zip)(包括binary和source) 

[musikCube下载](http://www.musikcube.com/page/main/download)
