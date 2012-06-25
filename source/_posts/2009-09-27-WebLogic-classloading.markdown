---
layout: post
title: "WebLogic的classloading"
comments: true
date: 2009-09-27 07:23
updated: 2010-02-22 23:18
tags:
- SoftwareDev
- WebLogic
- AppServer
---
一直对classloading的了解不是很清晰，每次遇到涉及WebLogic的classpath设置、classloading问题debug时都要去网上搜索资料。现在把资料整理下来。

[![wls_classloading](http://farm3.static.flickr.com/2682/4379354938_dc7cd5f47e_o.png)](http://www.flickr.com/photos/leoliang/4379354938/)

### 

###  各层级classloader加载的类的范围

* Bootstrap classloader
  * Core Java libraries (<jre>/lib)

* Extension classloader
  * JRE extensions directory (<jre>/lib/ext)

* WebLogic Server system classloader
  * Classpath
  * <domain>/lib

* Filtering classloader
  * nothing

* Application classloader
  * EJB JARs
  * APP-INF/lib
  * APP-INF/classes
  * Manifest Class-Path in EJB JARs

* Web application classloader
  * WAR
  * Manifest Class-Path in WAR

###  类加载的优先级

每个classloader在要加载一个class之前都会先请求它的上层classloader，如此逐级传递。因此效果就是越上层的classloader优先级越高，一个class如果在上层classloader能加载到，就不会让下面的classloader加载。

但是web application classloader是个特例，通过在weblogic.xml中配置<prefer-web-inf-classes>，可以让web application classloader优先在自己war范围内加载类，找不到的类才会请求上层classloader。

Filtering classloader是个特殊的classloader，它并不会加载任何类，而是起到控制类加载优先级的作用。在weblogic-application.xml中配置<prefer-application-packages>，可以限制对于指定的类不再向上层classloader请求，也就是限制在EAR的范围之内加载。

###  其他

以上仅仅描述了缺省情况下classloading的行为。WebLogic对于一个application (EAR)内的classloader的层次结构是可以自定义的。Shared library和optional package，也会影响到classloading。另外，resource adapter (RAR)也有独立的classloader。还有其他的一些细节，需要查阅[Understanding WebLogic Server Application Classloading](http://download.oracle.com/docs/cd/E13222_01/wls/docs100/programming/classloading.html)。
