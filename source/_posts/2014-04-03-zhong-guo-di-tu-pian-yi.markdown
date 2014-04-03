---
layout: post
title: "中國電子地圖偏移"
date: 2014-04-03 07:43:45 +0800
comments: true
tags: GPS_GIS 
---
幾年前我寫過對Google中國地圖偏移規律的分析([1](/2009/12/26/Google-China-Map-offset-1/))([2](/2009/12/27/Google-China-Map-offset-2/))，根據抓取到的偏移量數據，大概看出呈現了`sin(a)+sin(3a)`這樣的函數曲線規律，並且這個函數以不同頻率和幅度疊加了兩次。當時我推導出函數及其係數，利用它去校正偏移，在百公里範圍的區域內已經非常理想，但是在全國大範圍的不同區域，係數需要有點不同，也就是擬合函數還是差了一些低頻率的細節。不過當時的分析細節我也不敢在博客裏面寫，後來我也沒有再搞地圖方面的應用開發了，就把這個放下。

實際上這個地圖偏移並不是Google自己搞的算法，而是國家測繪局以立法的形式統一要求加的，稱為地形图非线性保密处理，所有從事國內電子地圖的廠商應該都得到了這個算法，實際上並起不到什麼所謂保護國家安全和國家利益的作用（保護行業壟斷者利益倒是真的），但是這個地圖偏移對地理信息處理和GPS技術的民用化起到了巨大的阻礙作用，近年各種與地理位置相關的應用如雨後春筍般湧現，但都為中國地圖的偏移而頭痛不已，後來由於下面提到的算法的流傳，問題得以解決，但增加很多不必要的複雜性和浪費開發、數據處理的人力。在我看來，這個地圖非線性加密處理在地理信息領域就像GFW在互聯網信息領域一樣，浪費了大量社會財富來對技術發展與應用進行封鎖。[李成名](http://baike.baidu.com/view/2356807.htm#2_2)，你真的成名了。

感謝Internet，感謝open source，現在這個偏移算法在網上已經可以輕松找到了。

網上流傳的原始算法是[這個Java文件](http://emq.googlecode.com/svn/emq/src/Algorithm/Coords/Converter.java)，然後有人整理過代碼，接力開發出多種語言版本。在『A Fork of Stuffs』博客裏，[地球坐标系 (WGS-84) 到火星坐标系 (GCJ-02) 的转换算法](http://blog.csdn.net/coolypf/article/details/8686588)給出了C#實現，這個博客另外一篇[火星坐标系 (GCJ-02) 与百度坐标系 (BD-09) 的转换算法](http://blog.csdn.net/coolypf/article/details/8569813)也很有用。而使用比較方便應該是下面的github代碼庫：

- [googollee/eviltransform](https://github.com/googollee/eviltransform): C, go, JavaScript, php語言版本
- [Leask/EvilTransform](https://github.com/Leask/EvilTransform): Java和C#語言版本，似乎就是將前面介紹的代碼收集到github裏面，沒有改動。

另外，github裏還有一個項目 [fourcels/lbs](https://github.com/fourcels/lbs) 是用查表法來糾偏的，裏面的偏移數據（共9M）說不定就是早期從Google地圖服務上抓下來的。但現在有了算法，查表法的價值就不大了。
