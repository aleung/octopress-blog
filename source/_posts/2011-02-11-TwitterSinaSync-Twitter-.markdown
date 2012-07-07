---
layout: post
title: "TwitterSinaSync，自动同步Twitter信息到新浪微博"
comments: true
date: 2011-02-11 05:31
updated: 2011-07-08 00:03
tags:
- MyWork
---
之前寻找同步twitter到新浪微博的方法，没有尝试到满意的。@cutemic[介绍说](http://cutemic.posterous.com/twitter)他是在GAE上跑一个叫twitter2sina的python程序，周期性读取自己的tweets，再调用API发送到新浪微博。

程序员的基本素质是懒惰，因此我问@cutemic他安装的程序是否支持多用户，如果支持，我就懒得自己折腾，让他给我加个用户好了。很可惜，他的程序是要将新浪微博用户名密码hard code到程序里的，那我只能自力更生了。

装起来之后，本着助人为乐的精神，考虑把这个服务也提供给别人使用。看看代码，调用新浪微博API是用Basic HTTP认证的，需要提供用户名和密码，这样太没有安全感了，别人不敢用的。如果改成OAuth，用户就不需要提供密码了。

动手尝试修改，于是，就有了这个[TwitterSinaSync](https://github.com/aleung/TwitterSinaSync)。

代码大概的历史：

  1. 原始版本是[twitter2sina](http://code.google.com/p/twitter2sina/)，没有使用新浪微博API，已经失效
  2. @cutemic的[修改版本](http://cutemic.posterous.com/twitter)，改成使用新浪微博API
  3. 我修改的[TwitterSinaSync]()

主要特性：

  * 部署于Google App Engine，免费、可靠、方便的云计算环境
  * 使用新浪微博API OAuth认证，用户不需要提供密码，保证安全
  * 支持多用户：多组twitter-微博同步绑定
  * 新用户凭管理员提供的邀请链接才能加入，方便私人分享

[代码](https://github.com/aleung/TwitterSinaSync)托管在GitHub，bug report 和 feature request可以往[这里]()提交。

_**Updated 2011-7-7:**_

由于新浪微博对API使用过滤异常的严格，这个应用基本上用处不大了，普通内容的tweet，大概十条里只有两三条能够成功同步，见[新浪微博API黑名单威武](http://good-good-study.appspot.com/blog/posts/124001)。如果应用能够通过审核，也许过滤会放宽一些，但是根据新浪微博API的用户协议，是不能用于从其他微博系统同步信息的，更何况是Twitter，通过审核的可能性微乎其微。故此我不再维护这个软件了，也不再提供邀请码，请谅解。
