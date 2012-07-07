---
layout: post
title: "配置samba文件共享"
comments: true
date: 2002-01-19 00:14
tags:
- SysAdmin
---
根据实践的经验，基本了解了samba配置方法，将它记录下来。   
  
一个基本的配置文件：   

``` ini  
[global]  
netbios name = SERVERNAME  
workgroup = GROUPNAME  
null passwords = yes (是否允许用户设置空密码,default=no)  
log file = /var/log/samba/log.%m  
max log size = 500  
dns proxy = No (default=yes,一般情况下没有必要)  
security = user (相当于WinNT/2000的文件共享)  
encrypt passwords = yes  
guest ok = yes (default=no,是否允许guest访问,即不需要输入密码)  
(在global节中的这个设定会作为各共享资源设定的缺省值)  
  
[homes] (每个用户会看到各自的home目录)  
comment = Your Home Directories  
read only = No  
browseable = No  
  
[share] (share to all, read only)  
comment = Share Folder  
path = /home/samba  
guest ok = yes (在global中设定了,这里可以不写)  
writeable = no  
  
[source] (leo,frank可读写;james只读;其余用户无权访问)  
path = /home/source  
valid users = james,leo,frank(可访问此资源的用户的列表,如果不指定,任何用户都可以访问)  
write list = leo,frank (有写权限的用户的列表)  
writeable = no (其他用户不可写)  
create mode = 666 (保证新建的文件目录可以其他用户读写)  
directory mode = 777  
force group = nobody (无论用户以什么身份login,强制为此user/group;不一定需要)  
force user = nobody  
```

在samba的权限控制中, 关键的是以下一些参数.   
invalid users   
valid users   
guest ok / public (两种写法都可以)   
这三个参数, 优先级从高到低, 控制用户是否可以/是否需要login到一个服务.   
write list   
read list   
readonly / writeable (两种写法都可以)   
这三个参数, 优先级从高到低, 控制用户是否有写权限.   
  
用户要访问一个服务(共享目录),  
首先要成功login到这个服务(或者这个服务是public, 不需要login),  
然后再根据配置决定他是否有写的权限. 只要login进入了,就起码有读权限了.   
  
另外, unix文件系统的权限设置是个前提, 无论samba的访问权限怎么设置,  
用户在unix中无权访问的文件在samba中也肯定是不能访问的.   

