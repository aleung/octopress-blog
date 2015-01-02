---
layout: post
title: "图解：Spring ApplicationContext in enterprise application"
comments: true
date: 2010-02-24 21:46
updated: 2011-01-20 02:29
tags:
- SoftwareDev
- SpringFramework
---
在一个比较复杂的enterprise application (EAR)里面，往往有多个war和ejb，如果希望使用Springframework通过IoC方式来管理及装配POJO beans，就要面对如何初始化application context，由谁来初始化，如何让这些war和ejb共享application context等问题。

关于多个war、ejb共享application context的方法，在下面这两篇文章里解释得比较清楚：

  * [Using a shared parent application context in a multi-war Spring application](http://blog.springsource.com/2007/06/11/using-a-shared-parent-application-context-in-a-multi-war-spring-application/)
  * [Using a Shared Context from EJBs](http://springtips.blogspot.com/2007/09/using-shared-context-from-ejbs.html)

在这里，我不再详细叙述上面文章中的内容，通过图解的方式希望能让它更易理解，需要对照阅读。

图中的方块、圆圈都表示实例，形状的嵌套表示它们的包含关系。在例子里，缺省值都显式写出来，方便查找。

[![share-context-1](http://farm5.static.flickr.com/4037/4384915548_2ec7d610e6_o.png)](http://www.flickr.com/photos/leoliang/4384915548/)

先看图中靛蓝色的box，是ContextSingletonBeanFactoryLocator实例，application context的共享是依靠它来实现的：对于每个locatorFactorSelector，在一个classloader的范围内只会存在一个ContextSingletonBeanFactoryLocator的实例。ContextSingletonBeanFactoryLocator内部有一个ApplicationContext（图中的天蓝色box），它的bean是由locatorFactorSelector所指向的xml文件所定义的。例如locatorFactorSelector为classpath*:beanRefContext.xml，就会寻找classpath中所有的beanRefContext.xml文件并根据它们去创建内部ApplicationContext。这个内部ApplicationContext只是其它ApplicationContext的容器，不存放application的bean。一个beanRefContext.xml文件应该像下面那样，里面是一个ApplicationContext，它才是真正的业务bean的容器。如果需要可以设置它的parent context。如果有多个beanRefContext.xml文件，就会创建多个ApplicationContext，它们通过id来标识。
    
``` xml beanRefContext.xml
    <beans>
        <bean id="a" class="org.springframework.context.support.ClassPathXmlApplicationContext">
            <constructor-arg>
                <list>
                    <value>context_a.xml</value>
                </list>
            </constructor-arg>
            <constructor-arg>
                <ref bean="b" />
            </constructor-arg>
        </bean>
    </beans>
``` 

EJB应该在onCreate()过程中通过下面的代码来获得application context，需要通过factoryKey（例子里是"a"）来指定具体是哪个context：
    
``` java
beanFactory= ContextSingletonBeanFactoryLocator.getInstance("classpath*:beanRefContext.xml").useBeanFactory("a").getFactory();
```

在war里是依靠ContextLoaderListener来启动ApplicationContext的。在web.xml里加入：
    
``` xml web.xml    
        <listener>
            <listener-class>
                org.springframework.web.context.ContextLoaderListener
            </listener-class>
        </listener>
```

这个application context包含的是/WEB-INF/applicationContext.xml中定义的beans。在Servlet中通过下面的代码可以获得这个context：
    
``` java
beanFactory = WebApplicationContextUtils.getWebApplicationContext(this.getServletContext());
```

但这样创建出来的是一个独立的application context，仅仅在war内部可以访问，无法做到多个war和ejb之间共享。

要实现共享，就要在web.xml中再加入：
    
``` xml web.xml
        <context-param>
            <param-name>parentContextKey</param-name>
            <param-value>b</param-value>
        </context-param>
        <context-param>
            <param-name>locatorFactorySelector</param-name>
            <param-value>classpath*:beanRefContext.xml</param-value> <!-- this is the default value -->
        </context-param>
```

这样，当ContextLoaderListener创建war的application context时，会根据locatorFactorySelector去取相应的ContextSingletonBeanFactoryLocator实例，并且从locator中根据parentContextKey拿到具体的context，将这个context作为war的application context的parent。在图中，war 1的servlet可以访问到key为'b'的context，因为它是当前context的parent。

最后的效果就是：哪个war或者ejb先初始化，它就会去初始化ContextSingletonBeanFactoryLocator实例，由于是singleton，只会有一个实例存在，所有的war和ejb都从这个locator里面拿到具体存放业务bean的context，实现了context的共享。

在这里需要留意的是classloader，必须很清楚的知道哪些类是由哪个层次的classloader加载的：所有共享的bean，以及相关的spring jar都必须由EJB classloader加载。关于EAR的classloader及层次关系请参看 [WebLogic的classloading](http://good-good-study.appspot.com/blog/posts/4218)。

在实际应用中，往往不需要对application context作隔离，整个ear里面所有ejb、war共享一个application context是最简便的。这时war里不需要定义任何context和context loader，与EJB一样直接使用ContextSingletonBeanFactoryLocator就可以获取到context。
    
``` java
beanFactory= ContextSingletonBeanFactoryLocator.getInstance("classpath*:beanRefContext.xml").useBeanFactory("share-context").getFactory();
```

[![sping_shared_context](http://farm6.static.flickr.com/5245/5370082919_6dfc7f7b1d_z.jpg)](http://www.flickr.com/photos/leoliang/5370082919/)
