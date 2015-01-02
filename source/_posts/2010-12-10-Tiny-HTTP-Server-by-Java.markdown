---
layout: post
title: "Tiny HTTP Server by Java"
comments: true
date: 2010-12-10 06:56
updated: 2011-11-08 21:50
tags:
- SoftwareDev
---
前两天在项目的测试环境中发现有一个Java实现的 Tiny HTTP server，就一个Java文件，不知道同事从哪里找来的。用来临时从服务器上下载些文件，或者查看HTML文档（例如 JUnit test report, JavaDoc）之类的非常方便。它只支持HTTP GET方法，URL必须指向一个文件。我顺手修改了一下，让它可以列出目录，这样用起来更加方便一些。

{% gist 1347769 %}

运行时可带两个参数：port, http root directory。例如:
    
    
    $JAVA_HOME/bin/java httpServer 8080 /tmp/test-report
    

P.S. 网上搜索一下，我的同事应该是从这里找到这段代码的：[http://www.java2s.com/Code/Java/Tiny-Application/HttpServer.htm](http://www.java2s.com/Code/Java/Tiny-Application/HttpServer.htm)
