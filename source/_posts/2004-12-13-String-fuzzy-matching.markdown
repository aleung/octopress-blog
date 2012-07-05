---
layout: post
title: "字符串模糊匹配"
comments: true
date: 2004-12-13 17:35
tags:
- SoftwareDev
---
字符串匹配最常用的是正则表达式，但是正则表达式的匹配非常严格，某些场合下，希望的匹配算法是模糊的，只要求找出相似的字符串而不是完全匹配的字符串，例如拼写错误、插入空格等情况。[http://www.codeproject.com/aspnet/intelligent404page.asp](http://www.codeproject.com/aspnet/intelligent404page.asp)这篇文章介绍了如何生成智能的404页面，当URL不存在时可提示出拼写近似的网页。其中使用Levenshtein的算法来计算两个字符串的‘edit distance’,也就是一个字符串要经过多少步修改（增加、改变、删除字符），才能变成另外一个字符串。根据计算出的相似的分值，据此来给出建议结果。

这个算法看来在处理用户输入的时候会很有用，能够增加用户界面的人性化程度。一个友善的提示，比起冷冰冰的“输入错误”友好多了，降低了用户使用软件时的心理阻碍。另外，这个算法应该也是可以适用于中文的，采用unicode处理就可以了。
