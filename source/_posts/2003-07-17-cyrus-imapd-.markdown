---
layout: post
title: "cyrus imapd的邮件目录修复"
comments: true
date: 2003-07-17 23:02
tags:
- SysAdmin
---
cyrus imapd的邮件目录修复：/usr/cyrus/bin/reconstruct, 邮箱quota查看与修复：/usr/cyrus/bin/quota，修复时要加 -f 参数。_注意：该命令必须以cyrus用户身份才能运行  
且此quota不同于/usr/bin/quota命令_  
  
/var/imap/quota/[a-z]/user.* 文件是用户的quota信息，第一行是当前占用的quota，第二行是quota limit。  
  
当用户有d权限时，才会在quota达到特定值时发送warrning信件。不过，正常使用的用户都是有d权限的。  
  
[http://asg.web.cmu.edu/cyrus/cyrus-overview.html](http://asg.web.cmu.edu/cyrus/cyrus-overview.html)
