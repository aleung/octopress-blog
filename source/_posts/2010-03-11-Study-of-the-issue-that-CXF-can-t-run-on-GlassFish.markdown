---
layout: post
title: "Study of the issue that CXF can't run on GlassFish"
comments: true
date: 2010-03-11 11:59
tags:
- SoftwareDev
---
##  Issue

SOAP WS client which uses CXF can't run on GlassFish AS. Getting ClassCastException on invoking some CXF API. The problem was described as CXF-2237 [2].

##  Analysis

JAX-WS is the Java API for XML Web Service defined by JSR 224. SUN provides a JAX-WS reference implementation in Metro project, which is inside GlassFish AS. The SUN JAX-WS implement was later also merged into JDK 6. Apache CXF is another JAX-WS implementation.

An application can select different JAX-WS implementation by Provider SPI. JSR 224 [4] chapter 6.2.1 defines the algorithm of Provider implementation selection:

> 1. If a resource with the name of META-INF/services/javax.xml.ws.spi.Provider exists, then its first line, if present, is used as the UTF-8 encoded name of the implementation class.
> 
> 2. If the ${java.home}/lib/jaxws.properties file exists and it is readable by the java.util.Properties.load(InputStream) method and it contains an entry whose key is javax.xml.ws.spi.Provider, then the value of that entry is used as the name of the implementation class.
> 
> 3. If a system property with the name javax.xml.ws.spi.Provider is defined, then its value is used as the name of the implementation class.
> 
> 4. Finally, a default implementation class name is used.

The Provider implementation class in JDK 6 is `com.sun.xml.internal.ws.spi.ProviderImpl` in rt.jar. I guess it's the default implementation as mentioned in step 4.

CXF packages META-INF/services/javax.xml.ws.spi.Provider which points to `org.apache.cxf.jaxws.spi.ProviderImpl` in cxf.jar. It will be considered in step 1.

Metro also packages META-INF/services/javax.xml.ws.spi.Provider which points to `com.sun.xml.ws.spi.ProviderImpl` in webservices-rt.jar. It also will be considered in step 1.

Because in GlassFish the Metro library is in system classpath, so its META-INF/services/javax.xml.ws.spi.Provider will be loaded precede over CXF's. That's why Metro will always be used as JAX-WS implement no matter CXF exists or not.

##  Potential Solution

###  Classloader control

If we can force the JVM to load resource from cxf.jar prior to webservices-rt.jar that will be fine. But GlassFish hasn't the mechanism likeWebLogic's `<prefer-application-packages>`[5]. After some finding we give up this approach.

###  Remove Metro from GlassFish

If there is no application which deploys on the this application server that depends on Metro, we can remove Metro lib (webservices-*.jar) from GlassFish's lib folder.

###  Change to Metro

That's nothing to say. We finally choose this way. There is almost no workload to migrate our code from CXF to Metro, because both of them follows JAX-WS specification, except that some code that calls CXF specify API need to be changed. We even don't change the build script and still use CXF's wsdl2java tool.

##  Reference

  1. [CXF Issue: CXF-857](https://issues.apache.org/jira/browse/CXF-857)
  2. [CXF Issue: CXF-2237](https://issues.apache.org/jira/browse/CXF-2237)
  3. [StackOverflow: How to pick CXF over Metro on Glassfish](http://stackoverflow.com/questions/2064068/how-to-pick-cxf-over-metro-on-glassfish)
  4. [JSR 224: JAX-WS 2.x](http://jcp.org/en/jsr/detail?id=224)
  5. [WebLogic的classloading](http://good-good-study.appspot.com/blog/posts/4218)
