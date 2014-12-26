---
layout: post
title: "Getting the Hang of Web Typography"
date: 2014-08-23 10:34:11 +0800
comments: true
tags:
- Reading
- Design
- UX 
---

读书笔记摘抄：《[众妙之门——网页排版设计制胜秘诀](http://book.douban.com/subject/25741532/)》

# 版式设计

正文字号常见为12~14px。

标题与正文字号（font-size）的比例，比较流行的平均值是1.96。也就是将正文文字字号乘以2，就得到标题文字的字号。字号从传统值（6,7,8,9,10,11,12,14,16,18,21,24,36,48,60,72）或者斐波那契数列（8,13,21,34,55）中选择，可以得到更自然的版式效果。

计算行宽的简单方法：行宽=字号*30。如果字号是10px，行宽就是300px，差不多等于一行65个字。

行高（line-height）受多种因素的影响：字体、字号、粗细、样式、行宽、字距等。行宽越宽，行高就需越大；字号越大，行高就需要越小。比较流行的行高与字号的比例是1.5。

段间距通常是行高的75%左右。

# 字体分类

网页设计中大多数字体可以分为五大类别：

### Geometric Sans 无衬线几何字体 

{%img /attachments/2014/8/geometric.jpg %}

这个类别实际包括了几何风格（Geometric）、现实风格（Realist）和奇异风格（Grotesk）三类字体。它们基于精准几何形体，每个字母笔画的宽度都是一样的，这种设计生动体现了“少即是多”的极简派的设计思想。

- 运用得当：干净、感性、现代而通用；
- 运用不当：冰冷、理性、枯燥。

_Examples of Geometric/Realist/Grotesk Sans:_ Helvetica, Univers, Futura, Avant Garde, Akzidenz Grotesk, Franklin Gothic, Gotham.

### Humanist Sans 无衬线人文字体

{%img /attachments/2014/8/humanist.jpg %}

由手写体衍化而来，它们中的一些可能看上去干净而现代，但仍旧保留了一些手写体的特点。一般具有更多细节、更少的一致性，而且字体较窄笔画偏粗。

- 运用得当：既现代又人文；
- 运用不当：缺乏力度，不够权威。

_Examples of Humanist Sans:_ Gill Sans, Frutiger, Myriad, Optima, Verdana.

### Old Style 古体字

{%img /attachments/2014/8/old-style.jpg %}

特点是粗细笔画间的对比微妙，还有字母的弧度朝左偏。

- 运用得当：古典，传统而易读；
- 运用不当：依然是古典和传统（与其他不协调）。

_Examples of Old Style:_ Jenson, Bembo, Palatino, Garamond(尤其显得传统)

### Transitional and Modern 过渡体和现代体

{%img /attachments/2014/8/trans.jpg %}{%img /attachments/2014/8/modern.jpg %}

这两类字体是启蒙思想的产物，字体设计师尝试改变平凡而低调的古体字的字形，使其变得更具几何特点、更锐利、更艺术。

- 运用得当：展现出力量感、时尚感和活力；
- 运用不当：不伦不类——若说古典，又过于显眼和繁复；若说现代，又显得有点俗气。

_Examples of transitional typefaces:_ Times New Roman, Baskerville.

_Examples of Modern serifs:_ Bodoni, Didot.

### Slab Serifs 带衬线的板式字体

{%img /attachments/2014/8/slab.jpg %}

笔画与其他衬线字体很像（形式简洁，笔画粗细对比相对微弱），但较为特别的是笔画末端有又方又硬的衬线。

这类字体可以传达出一种权威感，如Rockwell字体的粗体；也可以展现出友好的感觉，如Archer。独特的块状衬线能为版面带来特别的气息，不过一旦用错也尤为显眼。

_Examples of Slab Serifs:_ Clarendon, Rockwell, Courier, Lubalin Graph, Archer.

### Reference:

- [What Font Should I Use?](what-font)
- Making Sense of Type Classification ([part 1](type-class-1)) ([part 2](type-class-2))

[what-font]:http://www.smashingmagazine.com/2010/12/14/what-font-should-i-use-five-principles-for-choosing-and-using-typefaces/
[type-class-1]:http://www.smashingmagazine.com/2013/04/17/making-sense-type-classification-part-1/
[type-class-2]:http://www.smashingmagazine.com/2013/06/19/making-sense-of-type-classification-part-2/

# CSS Font Stack

创建字体栈的基本公式：最佳字体 + 次佳字体 + 常见的相似字体 + 相似的网页安全字体 + 通用字体。

在字体栈中要注意字体的字形比例。例如网页安全字体中，Verdana字形很宽，Arial/Helvetica相对较窄，它们不应该同时存在于版式中；Times和Georgia也是同理。

常见的字体栈参看 [Guide to CSS Font Stacks](http://www.smashingmagazine.com/2009/09/22/complete-guide-to-css-font-stacks/)
