---
layout: post
title: "系统性能监控工具"
comments: true
date: 2003-09-19 22:39
tags:
- SysAdmin
---
有些工具是专门用来绘制曲线图的, 可用于监控系统、设备的性能等用途，例如网络流量曲线/系统性能曲线等:  
  

* MRTG http://www.univ.kiev.ua/~roman/soft/mrtg/  
这个工具原本用于显示路由器的流量, 通过SNMP来获取流量. 但是也有扩展特性, 可以通过调用其他程序来采集数据并生成曲线图. 但由于原来设计是流量监测, 故此配置灵活性不如RRDtool.  
这里是各种扩展: [http://people.ee.ethz.ch/~oetiker/webtools/mrtg/links.html](http://people.ee.ethz.ch/~oetiker/webtools/mrtg/links.html)  

* RRDTool http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/  
RRD是Round Robin Database的意思, 这个工具专门用于收集数据, 并且按照一定的周期生成曲线图. 很象MRTG, 不过它不包含数据采集功能, 而且灵活性高适应面更广.  

* ORCA(http://www.orcaware.com/orca/docs/orcallator.html), 一个主机性能采集的工具, 利用RRDTool生成很多种性能曲线. 它只能在solaris上运行. 但它采集的数据和生成的曲线可以参考, 部分在linux上用RRDTool配合其他的采集程序应该也可以生成的.  

* BGraphs(http://www.ag0ny.com/graphs/) 利用RRDtool和shell script做的linux性能采集工具。我在它的基础上修改，安装到公司的上网服务器上，还不错。  

