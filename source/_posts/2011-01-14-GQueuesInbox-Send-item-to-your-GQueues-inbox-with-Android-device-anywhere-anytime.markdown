---
layout: post
title: "GQueuesInbox : Send item to your GQueues inbox with Android device anywhere anytime"
comments: true
date: 2011-01-14 05:15
updated: 2011-01-17 05:16
tags:
- Android
- TimeManagement
---
{% img right http://farm6.static.flickr.com/5285/5360976023_928916d839_m.jpg GQueueInbox %}
去年底[RememberTheMilk](https://www.rememberthemilk.com/)的pro帐号到期了，于是又折腾了一下看看有没有好用的类似工具，发现了一个在线任务管理的新秀：[GQueues](http://www.gqueues.com)。GQueues的功能和界面都不错，我比较喜欢它的一点是没有用高中低的任务优先级，而是让你在列表中拖放任务改变顺序，排在最顶的就是需要优先处理的。有些事情你很难界定应该是属于重要性高还是重要性中等，但是如果几件事情放在一起，就很容易分出哪个相对更重要一些。

但是，GQueues没有开发手机本地应用，在手机上只能通过浏览器访问。虽然它的网站有专门为移动设备优化，但还是不如本地应用便捷。而且很重要一点，不是任何时候都能访问到网络的，想起一件要做的事情，却因为连不上网而无法记录下来，只能留在脑子里或者要记录到别的地方，这样不符合GTD的原则。

为此，我写了一个简单的Android应用，做的唯一事情就是往GQueues inbox里面添加新任务，并且允许离线使用：如果没有网络，任务会暂存起来，在有网络的时候后台自动发送出去。软件介绍：[http://www.appbrain.com/app/gqueuesinbox/leoliang.gqueuesinbox](http://www.appbrain.com/app/gqueuesinbox/leoliang.gqueuesinbox)

不过，因为GQueues部署在Google App Engine上的，IP地址被GFW屏蔽，在国内无法直接访问。解决办法是自己将可以访问的IP配置到hosts文件里面，无论是PC还是手机都要如此处理。
