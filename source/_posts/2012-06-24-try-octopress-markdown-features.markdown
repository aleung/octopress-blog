---
layout: post
title: "试验 Octopress 的各种特性"
date: 2012-06-24 23:08
comments: true
categories: 
tags: Blogging
---

这个blog使用的markdown解释器是[kramdown](http://kramdown.rubyforge.org/quickref.html)，比起标准markdown有所增强。再加上Jekyll的一些插件特供的特殊功能。

Markdown基本语法
----------------------------

### 标题

标题用`#`开头，一个井号是一级标题，两个井号是二级标题，井号越多字体越小。

一级标题也可以通过在标题的下一行用`========`来标注，二级标题就是`---------`。更小的标题就不能用这种方式了。

### 链接

行内式链接：

    This is a [link](http://example.com).

参考式链接：在链接文字的方括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记（若省略则与链接文字相同），接着，在文件的任意处，把这个标记的链接内容定义出来。

    This is [an example][1] reference-style link.

    [1]: http://example.com

参考式链接可以提高 markdown 文本的易读性。

### 图片

与链接类似，区别是前面增加叹号`!`：

    ![text](link)

另一种图片语法是由插件支持的，用`{` `%`包围的img标签，后面跟着图片URL，在URL前可以加入可选的css class名称，如：`left`, `right`，得到文字环绕效果。

### 引用 (blockquote)

用`>`开头的一个段落：

> Stay hungry...
stay foolish.

用四个空格缩进的段落，会按原始格式显示，相当于HTML的`<pre>`的效果：

    +----+
    |    |
    +----+

也可以通过在原始格式引用段落的前后各加一行波浪号`~~~~~~`来实现。(这是kramdown特有的语法)

### 显示效果

用`*`或`_`包围的文字会用斜体显示：

Some of these words *are emphasized*.

如果双重符号`**`或`__`则会用粗体显示：

Use two asterisks for **strong emphasis**.

用`` ` ``包围的文字按代码格式显示。

### 列表

用`*`, `+`, `-`开头的行都会作为列表项。子项缩进两个空格。

+ 加号开头的段落
  + 缩进两个空格，加号开头
    + 再缩进两个空格，三级项目
- 其次

表格
------

注意第二行的分割线的冒号位置，决定了这一列的对齐方式。

    No.   | Name    | Status  |
    -----:|:--------|:-------:|
    1     | Alaph   | done    |
    2     | Beta    | ongoing |
    10000 | Release | n/a     | 

效果：

No.   | Name    | Status  |
-----:|:--------|:-------:|
1     | Alaph   | done    |
2     | Beta    | ongoing |
10000 | Release | n/a     | 

### 脚注

增加脚注[^1]很简单，就是这样：`[^1]`

脚注定义的写法是以`[^1]: `开头，后面跟着定义。

[^1]: 脚注一定会显示在最末尾。

嵌入代码
-----------

下面的嵌入代码的方式都是由插件提供的，并非markdown语法。

### 代码高亮

~~~~~
 ``` [language] [title] [url] [link text]
 code snippet
 ```
~~~~~

效果：

``` java HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World");
    }
}
```

### 嵌入Gist

{% gist 2934603 %}

方法是写一个用`{` `%`包围的gist标签，后面带上id。

- - - -
