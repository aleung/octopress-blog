---
layout: post
title: "抓取Android系统上的网络通信"
comments: true
date: 2010-03-19 13:33
tags:
- Android
---
Android是基于Linux的系统，这是对开发者很方便的。今天手机上网不太正常，想到抓些包来看看，于是上网google一下，就找到教程了：[Debugging with Tcpdump](http://pdk.android.com/online-pdk/guide/tcpdump.html)。我的ROM没有预装tcpdump（据说cm的rom有），自己编译也太麻烦了，网上也可以找到编译好的包，搜索"tcpdump-arm"可找到。

有了tcpdump，要分析别人的应用干了些什么也方便多了。例如这是 Google Maps 获取network location的包：
    
    
    POST /loc/m/api HTTP/1.1
    Connection: close
    Content-Type: application/binary
    Content-Length: 496
    Host: www.google.com
    User-Agent: GMM/3.0 (dream ERE27); gzip
    
    ...."location,1.0,android,android,en_US... I.....g...........g:loc/ql......
    ..
    .1.0.Iandroid/google/passion/passion/mahimahi:2.1/ERD72/22132:user/release-keys.#2:l-7Yxu1UHhuUZlb5:F_iqbZnw84bV2J0q*.zh_CN2
    ....CMCC .(...
    .voicesearch..
    .com.diddo.diddo.diddo..
    .apps.maps..
    .com.lim.android.automemman"..
    ..
    ...I...... ..(.......$....I...... ..(.....I...... ..(.....I...... ..(.....I...... ..(.....I...... ..(."...I...... ..(.0..K"...I...... ..(.0..J"...I...... ..(.0..."...I...... ..(.0...(......
    
    
    HTTP/1.1 200 OK
    Content-Type: application/binary
    Content-Disposition: attachment
    Cache-Control: no-cache, no-store, max-age=0, must-revalidate
    Pragma: no-cache
    Expires: Fri, 01 Jan 1990 00:00:00 GMT
    Date: Fri, 19 Mar 2010 04:49:31 GMT
    X-Content-Type-Options: nosniff
    X-Frame-Options: SAMEORIGIN
    Content-Length: 188
    Server: GSE
    X-XSS-Protection: 0
    Connection: close
    
    ..............g...)...........`.Z... $...k.......fg./,
    .....,.."....cZ
    ....(.
    !'....S`..F...3..BP.n..j...`Q{..V..v...Z'yT;.j...]..Q+.U.d.Pm.,6..aj..j_....=..T...
    goB. .U....s7.......#
    )...
    

不过payload是binary的，没法直接看出里面是些什么东西。
