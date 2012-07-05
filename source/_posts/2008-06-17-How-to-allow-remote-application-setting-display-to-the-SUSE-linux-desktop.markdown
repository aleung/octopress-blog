---
layout: post
title: "How to allow remote application setting display to the SUSE linux desktop"
comments: true
date: 2008-06-17 20:59
tags:
- Software
- SysAdmin
---
1. Allow remote TCP connection to the X server. You need root privilege to config it. 

Just run gdmsetup. This will open a window with multiple tabs. In the Security tab, disabled the line that says "Deny TCP connections to the X server". Close the window and then log out of your current X session. You need to restart gdm for this to work.  The easiest way to do that is to go to a console login, login as root, change to run level 3 (just type "init 3") and then return to run level 5 ("init 5") to restart gdm.   
Ref: [http://www.graphics-muse.org/wp/?p=69](http://www.graphics-muse.org/wp/?p=69)

2. Add remote host into X server access control

Run xhost _remote_host_ip_

3. Set DISPLAY environment variable in remote host before execute the application

setenv DISPLAY _display_host_ip_:0.0
