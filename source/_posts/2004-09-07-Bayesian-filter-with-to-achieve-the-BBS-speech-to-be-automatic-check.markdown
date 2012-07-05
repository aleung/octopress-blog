---
layout: post
title: "用bayesian过滤来实现论坛发言自动检查"
comments: true
date: 2004-09-07 07:21
tags:
- SoftwareDev
---
Bayesian过滤已经广泛用于邮件系统的anti-spam功能中，通过统计分析出spam邮件词语的统计特征，实现自动识别。

根据这个思路，也可以将bayesian过滤用在论坛的发言检查中，以减少版主工作量。

考虑到要易于与现有论坛系统集成，发言过滤器可以做成webservice之类的服务，提供两个功能调用：

  * void train(String content, boolean isMatch) 
  * boolean filter(String content)

前者让发言过滤系统学习，分析content特征；后者让系统分析content是否应该被过滤，返回值也可以是一个浮点数，表示分析出是敏感内容的可能性（确定性）。

对于论坛系统，如果本来已经支持版主审核功能的话，改造很简单。用户发言，调用filter方法，如果返回false，直接设置为已审核状态，直接显示；如果返回true，设置为待审核状态，等待版主人工审核。版主审核时，将选择是否需要过滤此内容，调用train方法，让过滤器学习。

[Classifier4J](http://classifier4j.sourceforge.net/)可以用于过滤器的开发，已经提供了Bayesian分类功能，需要补充的是中文分词的实现。
