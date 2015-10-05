---
layout: post
title: "HTML presentation framework"
date: 2015-08-29 03:23:55 +0800
comments: true
tags:
- Presentation 
---

最近要做一个分享，需要做个有趣一些的 slides。工作上的 slides 都是用 PowerPoints 来做的，但 ppt 放网上分享不方便，而且也不 cool，因此琢磨着弄个什么来玩玩。开始打算想用 markdown 来写，一方面是习惯了，另外也够简便。几年前就曾试过用 markdown 写一个简单的 [slides](http://aleung.github.io/presentation/markdown/slides.html)。

上网搜索一圈，发现这种打着 “HTML presentation framework” 名号的工具还真多，近些年来 HTML5、CSS、JavaScript 的广泛应用，这些基于 HTML 的演示可以做得很炫，让人耳目一新，例如这个 [50 Years of the Rolling Stones](http://vizzuality.github.io/rollingstonesmap/) 和 [Mass of Planets](http://lightning.fm/massofplanets/#/) (要允许网页使用摄像头)。

比较了一下，感觉 [reveal.js](https://github.com/hakimel/reveal.js) 不错，也很受欢迎。后来又发现另外一个后起之秀 [bespoke.js](https://github.com/markdalgleish/bespoke.js)，影响力不够 reveal.js 大，但它的插件式设计更胜一筹。对了，这些工具基本上都是用 JavaScript/Node.js 等一套工具链的，前端相关的技术 js 独霸天下了。

先试了一下 bespoke.js。看到 reveal.js 能使用 [Leap Motion](https://www.leapmotion.com/) Controller 来手势控制，演示起来会比较出彩，正好同事有一个 leap motion，可以借来玩，但是却找不到 bespoke.js 支持 leap motion 的插件。查了一下，原来 leap 有 JavaScript API，在浏览器里面能够通过事件监听获得双手各个手指的位置和手势动作的信息，API 暴露出来的已经是处理后的双手模型了，应用用起来相当方便。于是就简单的 port 了一个 [bespoke-leapmotion](https://www.npmjs.com/package/bespoke-leapmotion) 插件出来。但说实在的，leap motion 这东西还是中看不中用，用手势切换页面根本就不如手指头按个按键快捷和省力，手势的辨别也不够精准（这方面应用在数据处理上是可以下些功夫的）。

{% img /attachments/2015/8/leapmotion.jpg %}

Bespoke.js 的 markdown 支持似乎还有些坑，总有这样那样的问题。而且在 markdown 在版面布局方面局限性也很大，做出来的 slides 显得单调了些，效果不够理想，于是又在看看直接用 HTML 来写会怎样。HTML 是足够灵活了，但是要设置排版布局又累死了，我对 HTML+CSS 也不是那么熟悉，有点想放弃用回 PowerPoint 了。 

正好这个时候，发现了 [slides.com](http://slides.com) 这个网站，它背后用的就是 reveal.js，但它的在线编辑器真心好用啊，鼠标拖放元素，功能简洁但又足够做出漂亮的效果了。就是它了！

{% img /attachments/2015/8/slides-editor.png %}

但是，我还是念念不忘 leap motion —— 演示的时候可以带来新奇的效果啊。Slides.com 自然不会有这偏门的功能，但它很良心的提供了 export to reveal.js，在上面编辑好的 slides 可以放回本地的 reveal.js 里面播放，免费用户都可以享受。嗯，看起来一切都好，可是，怎么在 slides.com 里面写好的 speaker notes 导出后都丢失了呢？查看导出文件的内容，似乎是 slides.com 内部处理 speaker notes 用的机制跟 reveal.js 不一样，存放的格式也不同了。还好，只要数据都在，还是有办法的，写个[程序](https://github.com/aleung/slides-export)来转换就行了。
