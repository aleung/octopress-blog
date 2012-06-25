---
layout: post
title: "migrate my blog to github"
date: 2012-06-25 23:40
comments: true
categories: 
tags:
- Blogging
---

个人blog再一次搬家，从 Google Application Engine (GAE) 搬到 github 上来。原来的[地址](http://good-good-study.appspot.com/)依然能够访问，但是不会再更新了，新文章都发到这里。Feed订阅地址不变，建议使用Google Reader订阅本站。

## 搬迁原因

上一次从blogbus搬到GAE，是因为忍受不了审查制度。GAE后来也被墙了，但是即使如此，我也不会为了读者数量而再使用国内的blog服务，在被审查的状态下写东西。

这次搬到github，最主要的动机是为了使用[Octopress](http://octopress.org/)这个blog framework。

### 好玩

这个是最重要的原因。写blog不是什么正事，本来就是兴趣而已。之前搬离blogbus，选择了GAE的原因就是尝试一下GAE和PlayFramework编程。这次就是为了尝试Octopress。

### 使用markdown写作

Octopress使用markdown语法。接触markdown语法是在github项目的README页面上，还有[StackOverflow](http://stackoverflow.com)里发帖用的也是markdown。刚开始觉得又是另一种wiki语法，但慢慢习惯了发现很好用：语法不太复杂，常用的格式不难记住；基本上常用的格式都覆盖了，实在不行还可以直接写HTML；原始文本的阅读性很好，不像HTML没法直接看；排版容易，文本编辑器就可以，而HTML的WYSIWYG编辑器生成的内容经常格式错乱需要手工再编辑。

### 访问速度提高

原来在GAE上的blog因为访问量少，服务进程闲置一段时间就会关闭，新请求进来要实例化新服务进程。我用的是PlayFramework，每次初始化都要等上十来秒左右，因此打开首个页面一定要等，体验很不好。

Octopress是在本地生成HTML页面后静态发布，速度自然不慢。不过缺省页面模版有些引用的资源下载速度不快，也有些可能被墙了，影响页面加载时间，下一步需要优化一下。

### 版本管理与备份

Octopress最好用的特性就是跟github集成，发布到github上。整个blog就是一个git project，天然就有版本管理了。放在github上，我也不需要考虑备份问题了。

### 目前没有被墙

目前github是可以直接访问的，因此可以方便大陆读者。不过这个是最次要的原因了，因为作为一个优秀的网络服务，github说不定哪天就被GFW给认证了。另外，前面说过页面里面嵌入的某些资源是被墙的，我还没有测试过对访问会带来什么影响。

## 搬迁过程

Octopress的搭建不算难，需要安装ruby和一些gem，按照文档指导，再Google一下就能搞掂。按照文档做了一些配置，也没什么问题。缺省不支持tag，找了两个[插件](https://github.com/robbyedwards/octopress-tag-pages)装上去。模版和theme都用缺省的，稍微改了一点点，反正不在乎有没有个性了，我自己做也不一定好看，以后再说吧。缺省style显示不出表格，也需要改一点css。网上的资料不少，遇到问题Google一下大都能找到答案。

然后就是旧文章的迁移了，我原来的blog能够将所有数据用json格式导出，现在就要将它转换成新系统的文件格式；原来的文章正文是HTML，需要转换成markdown语法。不懂ruby，于是找了个python写的转换工具[html2txt](http://www.aaronsw.com/2002/html2text/)，自己再写点脚本就好了。

问题还是有一些的：

Post的时间很奇怪：如果精确到秒，Octopress(其实是Jekyll)会认为这是UTC时间，在generate的时候变换为本地时区，页面里的帖子时间就加了8个小时；如果不精确到秒，只是分钟，那它就会当做本地时间而不做变换。我都不知道这是by design还是bug，不懂ruby看了下代码不得要领。后来我就把时间全部都改成只保留到分钟，绕开这个问题算了。

Ruby不支持中文URL，因此有几个问题：

1. 文章URL不能是中文
2. tag URL不能是中文
3. 文章中不能带有中文URL的链接。

对于1，可以不用标题而只是用id来做post URL，但我觉得permalink还是用标题更cool一些，将中文翻译成英文好了。Google translate的API不公开了，本来想用Bing translate的API，但它要用OAuth2认证，我嫌要写太多代码了，后来找到有道翻译的API，效果还行。

对于2，因为tag的URL与显示是一致的，不能象post permalink那样做。网上的一些解决方法都比较复杂，不能保证日后升级兼容性，我干脆全部用英文做tag算了。

对于3，搜索了一遍旧文章，只有一个链接是中文的，而且已经失效了，故此也不需要花功夫了。

最后，就是还需要人工去check一遍老文章，看看自动转换的格式有没有问题，顺便回顾一下以前写的东西，有需要修改补充的就更新一下。
