---
layout: post
title: "Dump beans information from live Spring application context"
comments: true
date: 2011-11-08 18:42
tags:
- SpringFramework
- SoftwareDev
---
To maintain the Spring application context in a large application is not an easy job. There may be dozens of beans, whenever you want to make changes, you have to be very clear on there dependency. After serveral rounds of update, some beans might be orphan, they're unused any more but you don't know.

Spring IDE provides bean cross reference view and bean graph view that can help to analysis the bean relationship. But what Spring IDE has is a static view of the beans. In our project the application context is complicated: it scans classpath for all bean definition XML files and load them; application context created in WAR package inherits parent application context which is defines in EJB classpath; some beans are marked with annotation and loaded by <context:component-scan>, rather than define in XML file. I doubt Spring IDE can show the dynamic bean view as it is in runtime. What's more, Spring IDE isn't available at our standard development environment.

Inspired by a StackOverflow [answer](http://stackoverflow.com/questions/5850639/how-to-keep-track-of-all-the-autowired-stuff-while-using-spring-ioc/5851872#5851872), I created an ApplicationContextDumper. Add it into application context, it will dump all beans and their dependencies in the current context and parent contexts (if any) into log file when the application context initialization finishes. It also lists the beans which aren't referenced.

{% gist 1347171 ApplicationContextDumper.java %}

Here is an example. We have two application contexts. Please be noticed that the ApplicationContextDumper has been added into applicationContext.xml.

{% gist 1347171 applicationContext.xml %}
{% gist 1347171 parentContext.xml %}

parentBeanFactory is the parent of myBeanFactory:

{% gist 1347171 beanRefContext.xml %}

In main, loads application context from beanRefContext.xml. 

{% gist 1347171 main.java %}

The application context is dumped in log:

{% gist 1347171 output %}
