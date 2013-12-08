---
layout: post
title: "拦截Android应用HTTPS通讯内容"
date: 2013-12-08 15:03
comments: true
tags: 
- Android
- SoftwareDev
---
昨天在捣弄 [miCoach](http://micoach.adidas.com/) 到 [Nike+](http://nikeplus.nike.com/) 的数据迁移，到了最后一步发现调用 Nike+ API 需要传送client_id和client_secret，这两个信息是用来认证客户端的，但Nike并没有公开开放API，因此无法申请到client_id。看 [tcx2nikeplus](https://github.com/angusws/tcx2nikeplus) 的作者说他是通过查看iPhone应用发送的请求来拿到这两个信息的。但是 Nike+ API 都是走HTTPS的，普通方式的截包看不到加密传输的数据。

要嗅探加密传输，必须通过[中间人攻击](http://zh.wikipedia.org/wiki/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB)的方式才行。上网搜索一下看看有没有现成工具，果然一搜就找到了：[Burp Suite](http://portswigger.net/burp/)。它是一个安全测试工具，功能好像有不少，我这里用它做代理，从中监控应用到服务器的通讯内容。

下面记录大致操作过程。

在PC上安装运行Burp，设置 Proxy - Options - Proxy Listeners，让它监听合适的地址和端口，并且选择 "Generate CA-signed per-host certificates"。

将浏览器代理指向Burp proxy，访问任意一个https地址，Burp这时是中间人，它会用自己的根证书(PortSwiggerCA)签发一个目标服务器的证书，替换了真正服务器的证书。浏览器应该会有安全报警，因为系统并不信任签发这个证书的CA。查看证书详细信息，选择根证书并且信任这个根证书，就会把PortSwiggerCA的证书加入到系统的信任列表中。

要将这个根证书装进Android，需要先从系统 key chain 里将它导出到文件（.pem格式），然后执行下面的命令将它转换为DER格式后缀为.crt的文件。

    openssl x509 -in PortSwiggerCA.pem -inform PEM –out PortSwiggerCA.crt -outform DER

接下来，将 PortSwiggerCA.crt 放入Android的sdcard，在系统安全菜单中安装证书。证书安装后，Android系统就会信任所有Burp签发的证书了。

在Android的WLAN设置代理指向Burp。但很多Android应用都不理会系统的代理设置，不使用系统指定代理，遇到这种情况就要用 [ProxyDroid](https://play.google.com/store/apps/details?id=org.proxydroid) 来设置GlobalProxry（手机要root）。设置好了，在Burp里面就能够拦截到Android应用的HTTPS通信明文了。

{%img /attachments/2013/12/burp.png %}

*注意：为确保安全，测试完之后要将 PortSwiggerCA 这个根证书从PC和Android系统信任列表中删除。如果不是临时使用，应该要用自己的证书代替Burp提供的证书。*

这个故事告诉我们，中间人攻击并不是那么复杂的事情。特别是终端应用开发者，不可寄望于通过加密传输来隐藏应用到服务器的协议细节，要逆向工程是很容易的。