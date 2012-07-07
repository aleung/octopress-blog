---
layout: post
title: "MySQL是不会理会字段的NOT NULL标记的!"
comments: true
date: 2003-08-18 22:58
tags:
- SoftwareDev
---
原来MySQL是不会理会字段的NOT NULL标记的!  
按照MySQL manual中1.8.5.2节的说法: 当插入的字段的值不符号要求的时候, 不会出错, 而是用一个最接近的"合理"值来替代. 对于NOT NULL的varchar字段, 替换的值是空字符串''. 这不是等于没有设定NOT NULL吗? 真是莫名其妙.  
只好在程序里面来控制了
