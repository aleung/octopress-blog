---
layout: post
title: "新浪微博API黑名单威武"
comments: true
date: 2011-03-13 01:40
tags:
- SoftwareDev
---
因为有些tweet没有同步到新浪微博，查看了 [TwitterSinaSync](http://good-good-study.appspot.com/blog/posts/111001) 的运行日志，发现了这样的记录：

[![](http://farm6.static.flickr.com/5057/5519631639_5b0593fd42_z.jpg)](http://www.flickr.com/photos/leoliang/5519631639/)

前一个tweet（绿色框）正常同步到了新浪微博，而接下来一个就失败（红色框），错误信息是黑名单用户。显然是tweet中的某些内容触发了黑名单机制，在这里估计是"twitter"。

一旦用户被列入了黑名单，接下来对这个用户的操作都会失败。看下面的例子，第一条是成功的，第二个失败后，接下来的tweet的同步都失败了，即使内容看起来绝无敏感词。其实我也猜不出第二条tweet中哪个词触发了加入黑名单，是"境外"？还是"mranti"？

[![sina_blacklist_1](http://farm6.static.flickr.com/5217/5519631597_e6064611fc_z.jpg)](http://www.flickr.com/photos/leoliang/5519631597/)

目前新浪微博API的自动触发黑名单是临时性的，过一段时间后用户就可以继续使用，不过不知道它内部有没有什么计数器什么的。为此，我修改了 TwitterSinaSync，当用户被列入黑名单后会暂停同步一段时间。这个版本还处理了用户撤销新浪API授权的情况，会自动将用户删除。用户可以用原邀请链接重新开通。请到[TwitterSinaSync@github](https://github.com/aleung/TwitterSinaSync)下载 2.3 版本源代码。
