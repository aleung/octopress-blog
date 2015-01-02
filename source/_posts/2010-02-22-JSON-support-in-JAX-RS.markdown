---
layout: post
title: "JSON support in JAX-RS"
comments: true
date: 2010-02-22 23:33
tags:
- SoftwareDev
---
JAX-RS is the Java API for RESTful web services. JSON is the most popular data binding of RESTful web service. In JAX-RS, Entity Providers supply mapping services between representations and their associated Java types. To support JSON representation, JSON provider is needed.

##  JSON support in Jersey and CXF

Both Jersey and Apache CXF support JSON data generated out of JAXB beans. Developer can generate JavatypefromXML schema (xsd) by JAXB ormanually write Java bean with annotations. Then the resource can support JSON representatons.
    
``` java   
       @XmlRootElement
       public class StatusInfoBean {
           public String status = "Idle";
           public int tonerRemaining = 25;
           public final Collection<JobInfoBean> jobs = new HashSet<JobInfoBean>();
       }
       
       @Path("/status")
       public class MyResource {
           StatusInfoBean statusInfoBean = new StatusInfoBean();
           {
               statusInfoBean.jobs.add(new JobInfoBean("sample.doc", "printing...", 13));
           }
    
           @GET
           @Produces("application/json")
           public StatusInfoBean getStatus() {
               return statusInfoBean;
           }
    
           @PUT
           @Consumes("application/json")
           public synchronized void setStatus(StatusInfoBean status) {
               this.statusInfoBean = status;
           }
       }
```

##  Cusomize the JSON provider

The behavior of the JSON provider can be customized. But the way of customization is different in Jersey and CXF.

The configuration described in this article targets on Jersey 1.0.2 or higher, CXF 2.2 or higher.

In Jersey, developer should implement a JAXB ContextResolver which creates JSONJAXBContext with JSONConfiguration.Different configurations can be set bybuilder pattern.

``` java    
       @Provider
       public class MyJAXBContextResolver implements ContextResolver<JAXBContext> {
    
           private JAXBContext context;
           private Class[] types = {StatusInfoBean.class, JobInfoBean.class};
    
           public MyJAXBContextResolver() throws Exception {
               this.context = new JSONJAXBContext(
                       JSONConfiguration.mapped()
                                          .rootUnwrapping(true)
                                          .arrays("jobs")
                                          .nonStrings("pages", "tonerRemaining")
                                          .build(),
                       types);
           }
    
           public JAXBContext getContext(Class<?> objectType) {
               return (types[0].equals(objectType)) ? context : null;
           }
       }
```

Further simplificationway can be found in [http://blogs.sun.com/japod/entry/configuring_json_for_restful_web](http://blogs.sun.com/japod/entry/configuring_json_for_restful_web)

In Apache CXF, to customize the JSON provider, developer can implement a new provider or configure the properties of org.apache.cxf.jaxrs.provider.JSONProvider.

Custom provider is registered by Spring configuration:
    
``` xml
<beans>
    <jaxrs:server id="customerService" address="/">
        <jaxrs:serviceBeans>
          <bean class="org.CustomerService" />
        </jaxrs:serviceBeans>
    
        <jaxrs:providers>
          <ref bean="jsonProvider" />
        </jaxrs:providers>
    
        <bean id="jsonProvider" class="org.apache.cxf.jaxrs.provider.JSONProvider">
          <property name="dropRootName" value="true"/>
          <property name="serializeAsArray" value="true"/>
          <property name="arrayKeys" value="true">
            <list><value>jobs</value></list>
          </property>
        </bean>
    </jaxrs:server>
</beans>
```

###  Drop root element / Root unwrapping

Drop root element means strip out the root element from JSON. In the following example the StatusInfoBean element is dropped.
    
``` json 
{"status":"Idle","tonerRemaining":"25","jobs":{"name":"sample.doc","status":"printing...","pages":"13"}}
```

In Jersey whether dropping root element is configured by JSONConfiguration.rootUnwrapping(). In CXF it is by JSONProvider.setDropRootElement().

###  Single value array

Single value List object can be serialized as array or non-array. See example:
    
``` json    
    {"rows":{"userid":"1621","name":"Grotefend"}}
    {"rows":[{"userid":"1621","name":"Grotefend"}]}
```

In CXF it's controlled by JSONProvider properties "serializeAsArray" and "arrayKeys".

###  Namespace

If the JAXB bean has namespace, whether to include namespace in JSON can be controlled in CXF by JSONProvider properties "ignoreNamespaces".

##  Nature JSON notation

Nature JSON notation is:

  * root unwrapping
  * single value List object serialized as array
  * ???

See com.sun.jersey.api.json.JSONConfiguration.Notation
