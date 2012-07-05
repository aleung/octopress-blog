---
layout: post
title: "如何在不支持中文的网站中输入中文信息"
comments: true
date: 2006-06-28 12:23
tags:
- Web
---
有些网站不支持汉字，输入的中文字符显示不出来，变成了问号。geocaching.com就是这样。这通常是在文字处理的某个过程中只能处理7bit字符，将汉字过滤掉了。

在HTML/XML中对于这种在字符集以外的字符的表示有一种特殊方法：字符实体(character entity)，用字符实体来代替一个字符，相当于编程语言里面的转义符。HTML字符实体都是用&开头，分号结尾的，又分为两种，命名字符实体(named character entity)和数字字符实体(numeric character entity)，命名字符实体数量不多，都是些常用的符号，例如&nbsp;表示空格；数字字符实体是将unicode字符编码的十进制或十六进制数字放在&和;之间，例如&#23383;(十进制)和&#x5B57;(十六进制)。这样，所有unicode字符都变成了7bit ASCII编码，如果在不支持汉字的网站上输入这样编码后的中文，就不会在处理过程中被过滤了，只要浏览器支持，就能够把这些字符还原显示出来。

怎样将一段中文变成HTML实体的表示方法呢？知道相关的名词之后就好办了，用“HTML entity convert”之类的keywords到google搜索，找到了以下工具：

  * [Unicode (UTF-8) to HTML entities converter](http://konieczny.be/unicode.html) (online)
  * [Unicode Characters to HTML Entities Converter](http://pioneer.stereo.lu/converter.html) (online)
  * [ASCII Converter](http://www.mikezilla.com/exp0012.html) (online) 
  * [10進、16進文字コードin HTMLユニコード](http://code.cside.com/3rdpage/jp/unicode/converter.html) (online) 
  * [BabelPad](http://www.babelstone.co.uk/Software/BabelPad.html) (Unicode Text Editor for Windows)

一般的话用在线的都可以了，如果希望用本地软件的，可以用BabelPad，注意要将字体设置成宋体或者其他中文字体，否则中文显示不出来的。输入或者粘贴一段中文进去，全选，然后在菜单中选择“转换”-“统一码转换成HTML字名”。

不过，输入HTML实体，网站是不会将它按照一个字符来处理的，而是一串ASCII字符，故此在搜索、切分的时候就会出现问题了。
