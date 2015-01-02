---
layout: post
title: "NoClassDefFoundError"
comments: true
date: 2011-11-24 17:35
tags:
- SoftwareDev
---
java.lang.NoClassDefFoundError 是个很讨厌的错误，你会发现明明它报告的class已经打包进应用里，没可能在classpath中找不到，然后百思不得其解。其实这个exception跟 ClassNotFoundException 不同，后者报告的类是真的找不到，而这个NoClassDefFoundError 的错误原因是：class loader无法加载这个类，因为它依赖的另外某些类无法找到。到底是什么类找不到？它不告诉你。

例如，看这个exception stack trace:
    
    
    Caused By: java.lang.NoClassDefFoundError: net/sf/cglib/proxy/Enhancer
            at org.springframework.aop.framework.Cglib2AopProxy.createEnhancer(Cglib2AopProxy.java:228)
            at org.springframework.aop.framework.Cglib2AopProxy.getProxy(Cglib2AopProxy.java:170)
            at org.springframework.aop.framework.ProxyFactory.getProxy(ProxyFactory.java:112)
            at org.springframework.aop.framework.autoproxy.AbstractAutoProxyCreator.createProxy(AbstractAutoProxyCreator.java:476)
            at org.springframework.aop.framework.autoproxy.AbstractAutoProxyCreator.wrapIfNecessary(AbstractAutoProxyCreator.java:362)
            at org.springframework.aop.framework.autoproxy.AbstractAutoProxyCreator.postProcessAfterInitialization(AbstractAutoProxyCreator.java:322)
            at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyBeanPostProcessorsAfterInitialization(AbstractAutowireCapableBeanFactory.java:407)
            at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1426)
            at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:519)
            at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:456)
    

cglib.jar是在classpath里的，真正错误原因是CGLIB所依赖的asm.jar不存在。

有什么好方法查出NoClassDefFoundError的root cause吗？我不知道。不用Maven管理依赖的后果啊。
