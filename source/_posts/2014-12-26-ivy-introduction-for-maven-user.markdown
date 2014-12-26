---
layout: post
title: "Ivy Introduction for Maven User"
date: 2014-12-26 01:11:52 +0800
comments: true
tags:
- SoftwareDev 
---

由于 sbt 使用 Ivy 做依赖管理，必须了解一下 Ivy 的基础知识。这里从熟悉 Maven 的用户的角度简单描述一下 Ivy。

## 共同点

Maven的功能要比 Ivy 多很多，Maven 既管 build 也管依赖，而Ivy 仅仅是负责依赖管理。但就在依赖管理方面，两者在概念、模型、功能等方面都还是比较相似的。

对于 artifact 的标识，都是使用 groupID, artifactID, version 三元组，不过 Ivy 叫它们为 organization, module name, revision。Ivy 兼容 Maven 2 metadata，可以直接使用 Maven 2 repository。

## Configuration / Scope

Ivy 有个重要的概念叫 configuration，在 Maven 里类似的对应是 scope，但是 Ivy 的 configuration 比 Maven 的 scope 要灵活得多。

一个模块可以有多个 configuration，每个 configuration 包含一组外部模块依赖的声明（实际的配置是反过来的，每个依赖声明会标识自己在那种 configuration 中生效）。Configuration 可以是跟 Maven scope 类似的 compile, runtime 和 test，也可以任意定义。例如，某个模块既可以支持 Oracle 也可以支持 MySQL，当使用不同数据库时，需要依赖的模块是不同的，就可以分别在 oracle 和 mysql 两个 configuration 中定义各自需要的依赖。

Ivy 的 configuration 在依赖传递管理方面，比 Maven 要强。除了可以定义在一个 configuation 中需要依赖某个模块，还可以定义依赖这个模块时，会使用它的哪个 configuration，只有指定的 configuration 中的依赖才会传递进来。这个机制称为 configuration mapping。

下面的例子中，声明了在 default configuration 中需要依赖 hibernate，并且包含 hibernate 的 proxool 和 oscache 这两种 configuration 中的依赖。

``` xml
<dependency org="hibernate" name="hibernate" rev="2.1.8" conf="default->proxool,oscache"/>
```

Configuration 可以扩展（继承）另一个 configuration，可以设置外部可见性。

## Repository

Maven 的 repository 必须是网络服务（local 除外，local repository 本质上相当于 cache），Ivy 除了支持Maven 2 repository，也支持指定文件系统路径作为repository，还有各种各样其他的存储方式。

Maven 当配置了多个 repository 时，是按照配置的顺序一一查找，找到 artifact 为止。Ivy 的 artifact 查找是由 resolver 来负责，一个 resolver 对应一个 repository，有些特殊的 resolver 可以将多个 resolver 组合起来使用，例如 chain resolver 和 dual resolver。缺省的 resolver 就是一个 chain resolve，按照 local, shared, public的顺序去访问这些 repository。Ivy 的组织要比 Maven 更灵活一些。
