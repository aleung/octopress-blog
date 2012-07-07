---
layout: post
title: "邮件系统发信缓慢问题的解决"
comments: true
date: 2002-03-13 00:13
tags:
- SysAdmin
---
公司的邮件系统, 从内部网(10.x.x.x)连接SMTP端口非常慢, 但使用webmail或者从internet连接就一切正常.  
  
检查发现缓慢是是smtpd对client IP进行DNS lookup引起的, 需要等待lookup超时.  
  
解决办法:  

* 确认邮件服务器使用本机作为DNS ( /etc/resolve.conf, /var/spool/postfix/etc/resolve.conf )  
* 在本机配置DNS服务, 在本机DNS上负责解析反向域 10.in-addr.arpa. 其余域名forward给外部DNS解析.  

