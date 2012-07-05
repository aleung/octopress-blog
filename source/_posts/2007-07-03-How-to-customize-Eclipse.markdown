---
layout: post
title: "How to customize Eclipse"
comments: true
date: 2007-07-03 21:18
tags:
- SoftwareDev
---
要定制一个Eclipse特别版本非常简单，由于它是完全"绿色"安装的，只需要解开原始发布包，修改里面的配置，重新打包即可。重新打包前，要将configuration目录中的子目录都删除，只保留config.ini文件。

添加plugin，通过菜单的Help - Software Updates - Find and Install进行。

对于eclipse网站下载的Eclipse SDK, 客户化配置应该修改plugins/org.eclipse.sdk目录里面的东西.

  * About窗口的内容对应于plugin.properties文件中的设置。(删除configuration目录内容后才能看到变化)
  * Splash screen 的图片所在目录是在configuration/config.ini的osgi.splashPath中指定的。应该是 plugins/org.eclipse.platform 目录中的splash.bmp。
  * Perference选项缺省值的定制在plugin_customization.ini文件中指定。在Eclipse的perference 对话框中将需要改变的值改好，然后在File:Export将perference export到一个efp文件。这个文件是properties文件格式的，用文本编辑器打开它，找出需要定制的项目，去掉前面的/instance/， copy到plugin_customization.ini文件中。

Reference:

  * Eclipse help : Platform Plug-in Developer Guide : Programmer's Guide : Packaging and delivering Eclipse based product
