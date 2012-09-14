---
layout: post
title: "成员函数调用delete this合法吗？"
comments: true
date: 2005-02-18 21:18
tags:
- SoftwareDev
---
Yesterday, I discussed with Alex whether "delete this" in C++ is legal code.

In some case, only the object itself knows when its life is to the end and should be destroyed, the owner of object doesn't know it, or the object has no explicit owner. The most easy way to manger the life time of this kind of object is let it delete itself.

But we doubted that "delete this" isn't legal in C++: after the object is released, how does the execution continue?

``` c++
class A {  
public:  
  void foo();  
};

void A::foo()  
{  
  // do something  
  delete this;  
  cout << "I commit suicide!" << endl;  
}

int main()  
{  
  A* a = new A;  
  a->foo();  
}
```

I find the answer now, it's legal, but must be careful when using it. 

The following [faq](http://www.sunistudio.com/cppfaq/freestore-mgmt.html#[16.14]) explain it clearly:

>只要你小心，一个对象请求自杀(`delete` `this`).是可以的。
>
以下是我对“小心”的定义：
>
  1. 你必须100%的确定，`this`对象是用 `new`分配的（不是用`new[]`，也不是用[定位放置 `new`](http://www.sunistudio.com/cppfaq/dtors.html#[11.10])，也不是一个栈上的局部对象，也不是全局的，也不是另一个对象的成员，而是明白的普通的`new`）。
  2. 你必须100%的确定，该成员函数是`this`对象最后调用的的成员函数。
  3. 你必须100%的确定，剩下的成员函数（`delete` `this`之后的）不接触到 `this`对象任何一块（包括调用任何其他成员函数或访问任何数据成员）。
  4. 你必须 100%的确定，在`delete` `this`之后不再去访问`this`指针。换句话说，你不能去检查它，将它和其他指针比较，和 `NULL`比较，打印它，转换它，对它做任何事。
>
自然，对于这种情况还要习惯性地告诫：当你的指针是一个指向基类类型的指针，而没有[虚析构函数](http://www.sunistudio.com/cppfaq/virtual-functions.html#[20.4])时（也不可以 `delete` `this`）。 

And these ariticles also take about "delete this":

[http://info.tlw.cn/6079.htm](http://info.tlw.cn/6079.htm)  
[http://blog.csdn.net/foxmail/archive/2004/09/22/113060.aspx](http://blog.csdn.net/foxmail/archive/2004/09/22/113060.aspx)
