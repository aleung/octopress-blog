---
layout: post
title: "Invalid XML character, msxml.dll的问题"
comments: true
date: 2003-09-11 22:35
tags:
- SoftwareDev
---
Java程序测试中发现parse一个XML文件时出现exception, 错误信息为invalid XML character. 但这个XML文件用XMLSpy检查却没有问题.  
  
经过查找资料, 发现是XML文件中包含了一些不允许在XML中使用的字符(控制符). XML规范中规定了允许的字符范围([http://www.w3.org/TR/REC-xml#dt-character](http://www.w3.org/TR/REC-xml#dt-character)):  
  
    Char ::= #x9 | #xA | #xD | [#x20-#xD7FF] | [#xE000-#xFFFD] | [#x10000-#x10FFFF]   
  
包含illegal字符的XML文件不是well-formedness的, 为什么这样的文件能够生成, 并且XMLSpy不报错? 是Microsoft XML parser的问题. 这个XML文件是使用msxml.dll生成的, 估计XMLSpy用的也是msxml.dll, 这个parser的其中一个缺陷是允许某些非常字符的存在. 而Java程序中用的parser估计是sun的crimson, 这个parser严格检查XML character range, 包含非法字符就会报错.   
  
msxml.dll的问题看来还有不少, 这里专门有谈到: [http://www.xml.com/pub/a/1999/11/parser/index.html?page=2](http://www.xml.com/pub/a/1999/11/parser/index.html?page=2)   
  
为了解决此问题, 需要在生成XML前先对字符串检查, 把非法字符过滤掉. 这里提供了java的过滤函数: [http://lists.ibiblio.org/pipermail/xom-interest/2003-May/000471.html](http://lists.ibiblio.org/pipermail/xom-interest/2003-May/000471.html)   

