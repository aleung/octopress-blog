---
layout: post
title: "语音浏览技术规范简介"
comments: true
date: 2004-07-10 00:59
tags:
- SoftwareDev
---
一直以来，在CTI领域语音应用的开发都是采用各厂商的专有技术的，每个平台厂商都伴随自己的平台推出一套语音业务开发的规范，有些是脚本式的语言，有些是图形化的流程，这些规范受限于平台，相互之间没有兼容性，基于这些专有规范开发出来的语音业务很难从一个厂商的平台移植到另一个厂商的平台。

另外一方面，这些专有的语音业务开发规范的体系是非开放性的，业务控制局限在语音平台内部，与外界系统进行交互只能通过有限的途径，例如访问数据库、数据访问网关，开发COM接口组件等。造成难于与外部系统有机集成，集成成本高。在业务开发上，也因为业务处理逻辑与用户交互控制混合在一起，使得复杂业务的开发难度和维护难度都比较高。

近年来，随着网络技术的发展，各种业务应用都纷纷往网络方向发展，充分利用internet的数据自由流动和协议标准化的优势，CTI技术与web技术融合的需求越来越大。基于web的各种开发技术也迅速发展并成熟，包括J2EE、.NET、WebService等等，web应用开发渐渐变得快捷而高效。另外一方面，随着手机、PDA等手持设备的发展，对于延伸使用者界面，多模式互动的需求越来越多，提供键盘、笔输入、语音等多种输入手段，各种文字、影音输出途径，语音应用和传统文字/图形应用的界限越来越模糊。

在这样的趋势下，业界研究推出了多种涉及语音技术的标准规范。其中，W3C（World Wide Web Consortium）走在前面，其下的语音浏览器工作组等多个工作组进行的标准规范制定工作都涉及了语音技术。目前，对于电话和语音应用领域，重要的规范有三个，分别为VoiceXML, CCXML(Call Control eXtensible Markup Language), SALT(Speech Application Language Tags)。

这三个规范都是基于XML的，这是因为XML作为一种可扩展的通用标记语言，有着标准化、结构化的特点，并且对于XML的生成、传输、解析、验证、查询都已经有一系列相当成熟的技术和编程开发包，存在着很大的优势。但XML本身并不说明什么，它只是用来描述规范的一种语言，支持XML跟支持VoiceXML、CCXML这些规范是完全两回事。

这三个规范应用在系统中，部署架构基本是相同的，从高层次来看，由两个主要模块构成：文档服务器（document server）和电话语音平台（speech/telephony platform）。文档服务器由web server、database server、application server等构成，可以使用J2EE或者.NET平台。业务应用部署在文档服务器上，它响应电话语音平台发送来的请求，生成XML规范文档。电话语音平台包括了解释器、TTS、ASR等部分，它解释执行文档，负责与用户的交互界面。

电话语音平台与PSTN接口，或者提供VoIP支持。当一个呼叫进入系统，电话语音平台分析出业务类别，通过HTTP协议向文档服务器发起请求。文档服务器执行业务应用，生成VoiceXML或者CCXML规范的文档，返回给电话语音平台。电话语音平台内置了VoiceXML或者CCXML浏览器，解释执行文档内容，控制ASR与TTS操作，与电话用户进行交互。TTS服务器将文字合成为语音，播放给用户；ASR服务器接受用户的语音输入，利用语法规则(grammar)将用户说话内容识别为文本数据，平台在脚本控制下根据输入内容判断下一步的执行。

大多数情况下，基于web的应用都会采用易于扩展的架构，将核心服务逻辑（业务逻辑）与表示细节（VoiceXML, CCXML, SALT, HTML, WML）分离开。某些场合下还会将应用对话状态的维护与表示层分离，以实现表示语言机制的无关性，这样同一个应用可以采用web（HTML）、wap（WML）、语音（VoiceXML/CCXML）等不同的表示形式，适应PC、PDA、电话等多种用户终端。

使用这些XML系列标准技术规范的系统与过去传统专有规范的系统在架构上有所不同，使用标准规范的系统，业务的部署与平台的部署是分离的，相互通过HTTP协议松耦合，业务采用URL进行定位。这使得业务的分布部署变得非常简单。业务的开发采用web应用开发完全相同的模式，使得语音业务开发人员可以充分利用web应用开发的技术和经验。并且，用较小的代价就可以实现语音应用与web应用集成，或者实现应用的多种表示形式，适应不同的客户终端。

#### VoiceXML

VoiceXML可以理解为另外一种表示语言，类似于HTML和WML。它是一种表述对话（dialog）的语言，用来控制业务过程中的人机交互过程，适用于面向电话、手机等终端设备的语音应用，例如自动客户服务、自助查询系统、个人消息系统等。

将VoiceXML与HTML对比，就能很容易理解了。浏览器解释后，HTML表示的内容是以文字图像方式显示在屏幕上的，VoiceXML的内容是以语言的方式播放给用户的。HTML接收用户的文字输入和鼠标点击，VoiceXML接受用户的语音输入，进行语音识别，或者是通过电话按键输入DTMF数据。

VoiceXML是一种独立的语言，不能内嵌到现有的web语言中（如HTML，WML）。

``` xml
<?xml version="1.0"?>  
<vxml version="2.0">

<catch event="event.exit">  
<exit />   
</catch>

<menu id="choose_menu_option">  
<prompt>  
You are in a demo for content services.  
To obtain stock quotes, say stocks or say 1.   
For weather information, say weather or press 2.  
For news, say news or press 3.  
</prompt>  
<choice dtmf="1" next="#stocks_menu_option">stocks</choice>  
<choice dtmf="2" next="#weather_menu_option">weather</choice>  
<choice dtmf="3" next="#news_menu_option">news</choice>  
<nomatch>  
<prompt>I could not understand what you said, please try again</prompt>   
<reprompt/>  
</nomatch>  
<noinput>  
<reprompt/>  
</noinput>  
</menu>

<form id="stocks_menu_option">  
<block>  
...  
</block>  
</form>

<form id="weather_menu_option">  
<field name="city">  
<grammar type="application/srgs+xml" src="cities.grxml"/>  
<prompt>Pleasesaythe city's name</prompt>  
</field>  
...  
<filled>  
<submit next="http://host/weather.do"/>  
</filled>  
</form>

<form id="news_menu_option">  
<block>  
...  
</block>  
</form>  
</vxml>
```

从例子可以看出，VoiceXML描述的是一个个对话，menu和form是对话的两种形式。menu是列出选择项，供用户选择；form是播放提示语言，接受用户输入。

&lt;prompt>、&lt;audio>控制语音输出，输出有两种方式，一种是利用TTS将文本合成为语音，另一种是播放预先录制好的语音文件（如vox，wav）。为了控制合成语音的过程，提供丰富的语音表现形式，要求VoiceXML解释器支持SSML（Speech Synthesis Markup Language）语言，SSML可以控制TTS如何合成语音，例如重音、停顿、声调、数字播报方式等。

语音输入的识别是通过语法规则控制的，&lt;grammar>指定输入项采用什么语法规则进行识别，语法规则要求使用SRGS(Speech Recognition Grammar Specification)规范的格式来描述。

VoiceXML的执行顺序是有内置的规则的，称为FIA（Form Interpretation Algorithm），解释器根据FIA选择对话，执行对话中的输入、输出操作。为了提供对例外情况的控制手段，改变执行流程，VoiceXML具备事件机制，通过&lt;throw>, &lt;catch>, &lt;noinput>, &lt;error>,&lt;help>, &lt;nomatch>等元素可以抛出事件和捕获事件。

通过&lt;goto>、&lt;submit>等控制元素，能实现对话之间或者文档之间的转移。

在VoiceXML中，可以嵌入ECMAScript(即通常说的JavaScript）代码，实现客户端的运算功能。

VoiceXML提供了一些机制实现扩展和重用，&lt;object>元素供实现平台定义的功能，&lt;subdialog>可以实现语音组件的重用。

VoiceXML的目的是用来控制语音方式的人机交互过程的，因而它缺乏对呼叫的控制能力。例如会议、呼叫控制、建立呼叫、拒绝呼叫等等都无法实现。

#### CCXML

针对VoiceXML在呼叫控制方面的不足，CCXML补充了这部分的功能。它能够发起呼叫、过滤和路由进入的呼叫、处理外部的异步事件。并且它能支持多方会议，可以将VoiceXML实例作为参与者加入会议，并控制VoiceXML的执行和停止。与VoiceXML类似，由CCXML浏览器负责解释执行CCXML文档。

CCXML可以独立使用，但很多情况下是与VoiceXML配合使用，CCXML控制整个呼叫模型，VoiceXML控制每个呼叫中的用户交互。单纯的VoiceXML是无法实现电话QQ之类的涉及到多方通话的业务的，利用CCXML就有可能实现了。

_（未完成）_

_原来计划写成一篇介绍VoiceXML、CCXML和SALT，并且比较异同点的文章的，后来因为工作安排的原因半途而废了。CCXML日后如果需要，可能会进一步研究，到时也许会补上；SALT在我的工作中不会涉及，不打算在上面花时间了。_
