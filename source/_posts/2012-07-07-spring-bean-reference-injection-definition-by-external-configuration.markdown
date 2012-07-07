---
layout: post
title: "Spring bean reference injection definition by external configuration"
date: 2012-07-07 21:38
comments: true
tags:
- SpringFramework
- SoftwareDev 
---
当使用 Spring 作为 IoC framework 的时候，有时会利用 property placeholder 来将bean的注入属性做成可配置化。但是，一般很少会将 reference bean id 也用 property placeholder 代替，有人甚至以为这是不允许的。

利用Spring Framework的property placeholder，将bean依赖关系配置抽离Spring XML文件，就能在产品发布部署后，通过配置选项来选定不同的实现，可用于集成接口、特定业务逻辑打开关闭等。

[Adaptor pattern](http://www.oodesign.com/adapter-pattern.html) is widely used at the point where our product integrates with external system. Product will provide several adapters to adapt to different external system interface.
We deliver our product in uniform installation package for all customers. A mechanism is required to configure which adapter to be used after system is installed.

[Factory pattern](http://www.oodesign.com/factory-pattern.html) can be used to create the specific adapter by configuration property. But by this way we need to create factory for each kind of adapters.

Actually Spring Framework already supports it. With property placeholder, it is able to use placeholder in bean reference and resolve the bean name from properties. The bean definition XML file is not allowed to be modified when product is released, because it's packaged in war/ear. But properties can be modified as long as it's store outside of the package, e.g. on file system or in configuration management (extend the PropertyPlaceholderConfigurer class).

Here is a sample code snippet:
{% gist https://gist.github.com/2934603 %}