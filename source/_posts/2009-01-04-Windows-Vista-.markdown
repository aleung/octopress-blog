---
layout: post
title: "Windows Vista无法登录招商银行网上银行的问题"
comments: true
date: 2009-01-04 06:27
tags:
- Software
---
手提电脑升级到Vista操作系统后，在家里通过ADSL上网，招商银行网上银行无法登录，报告通信出错。运行自带测试程序，发现是与服务器无法进行SSL通信。但是在办公室上网时登录招行网银又很正常。打招行的服务热线，客服让我设置了IE的一些SSL相关选项，没有用。因为办公室网络可用，根据中国电信一贯的不佳记录，我第一时间就是怀疑电信封了ADSL的某些端口，造成ADSL无法使用招行网银。向中国电信投诉，接case的小伙子检查来检查去，都没有发现问题，他说帮我把局端可以换的东西都换过了，设置也全重设过，但都没有用。

跟同事说起，有人告诉我是IE7新增的一个安全选项的影响，将Advance Settings - Security - Check for server certificate revocation选项disable就可以正常使用招行网银了。回家一试，果然就正常了。可是，为什么呢？上网查了一下certificate revocation check的作用，大概的意思就是，SSL通信建立时，服务器需要有有效的安全证书，这个证书是由认证中心(CA)签发的，但是由于某种原因CA会撤销某些已经签发的证书，当IE的这个选项打开时，浏览器就会去CA检查一下服务器的证书是否已经被撤销。那么，看来还是招商银行的问题了，**为什么招行网银服务器的证书会在已撤销列表中？这应该是个很大的安全问题啊**。

至于为什么同样打开Check for server certificate revocation选项，在办公室网络可以正常访问招行网银，我估计那是由于办公室上网要经过proxy，很可能是公司内部的proxy屏蔽了CA的访问，造成浏览器无法得知证书已被撤销。

用Wireshark抓网银登录时的网络流，发现当选项打开时，确实出现了一个HTTP GET [http://crl.verisign.com/Class3InternationalServer.crl](http://crl.verisign.com/Class3InternationalServer.crl)请求，请求者是Microsoft-CryptoAPI/6.0
