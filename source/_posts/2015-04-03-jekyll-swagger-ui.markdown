---
layout: post
title: Jekyll 与 Swagger UI 集成 
date: 2015-04-03
comments: true
tags:
- SoftwareDev 
---

[Swagger](http://swagger.io/) 是被广泛使用的用于 REST API 描述和文档化的框架。它制定了一个用于描述 REST API 的规范，提供了一组工具用于编辑 API 描述文件、生成测试客户端、生成文档等等。[Swagger UI](https://github.com/swagger-api/swagger-ui) 是其中的用于生成文档的工具。

[Jekyll](http://jekyllrb.com/) 是深受程序员喜爱的静态网站生成工具，通常用来做 blog 站点，我们的工作团队也[用它来做内部的技术文档库](/blog/2015/01/26/jekyll/)。

在 Jekyll 里描述我们产品的 REST API，编辑排版是比较繁琐的事情，不同的 API 描述也没有统一的风格，因此考虑到引入 Swagger 来规范化 API 定义。

为此，写了一个简单的 [Jekyll 插件](https://github.com/aleung/jekyll-swagger-ui)，只要像下面例子那样在 markdown 中简单的插入 `swagger` tag，指定 API 描述文件，相应的 API 文档就会由 Swagger UI 生成并嵌入在页面中。

{% raw %}
    {% swagger /api/my-api.json %}
{% endraw %}

安装使用方法请参看 [README](https://github.com/aleung/jekyll-swagger-ui)。另外，Swagger UI 没有考虑到在一个页面中显示多组 API，会有些显示问题，已经提交 pull request，下个版本有望修复，暂时可以先下载我的修改版本，详见 README。


