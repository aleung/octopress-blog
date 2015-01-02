---
layout: post
title: "如何让JTextArea自动滚动到最底?"
comments: true
date: 2003-09-04 22:52
updated: 2012-06-18 23:35
tags:
- SoftwareDev
---
如果用JTextArea来做信息窗口，不断用append()显示新信息，通常会希望内容能自动滚动，保持最后增加的信息能够显示出来。利用setCaretPosition()可以实现，这个方法是设置输入光标的位置，如果光标位置超出目前可视范围，会自动滚动以保正光标可以显示出来。
    
    
    int length = textArea.getText().length();
    textArea.setCaretPosition(length);
    

如果还希望限制JTextArea中的文本长度，不让它无限制的增长，可以加上以下两行，当长度超出预订的maxTextLength后将开头的字符截除。不过美中不足的是截除无法按行进行。
    
    
    if ( length > maxTextLength ) { 
        textArea.replaceRange("", 0, length - maxTextLength); 
    }
    
