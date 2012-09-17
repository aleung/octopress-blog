---
layout: post
title: "Java的volatile关键字的作用"
comments: true
date: 2012-9-17
tags:
- SoftwareDev
- Java
---

- Initial: 2008-12-04
- Updated: 2012-9-17

Java内存模型中，有主内存和每个线程各自的工作内存，虚拟机和硬件可能会让线程工作内存优先存储于寄存器和高速缓存中，以提高性能。

所有变量都存储在主内存中，线程工作内存中保存了此线程使用到的变量的副本。工作内存在线程之间是隔离的，对其他线程不可见。线程对变量的所有操作都必须在工作内存中进行，修改后的变量副本要写回主内存。这样就会出现同一个变量在某个瞬间，在一个线程的工作内存中的值可能与另外一个线程工作内存中的值，或者主内存中的值不一致的情况。

一个变量声明为volatile，就意味着这个变量被修改时其他所有使用到此变量的线程都立即能见到变化（称之为可见性）。具体是在每次使用前都要先刷新，以保证别的线程中的修改已经反映到本线程工作内存中，因此可以保证执行时的一致性。以下例子展现了volatile的作用：

``` java    
    public class StoppableTask extends Thread {  
      private volatile boolean pleaseStop;  
      
      public void run() {  
        while (!pleaseStop) {  
          // do some stuff...  
        }  
      }  
      
      public void tellMeToStop() {  
        pleaseStop = true;  
      }  
    }
```

假如pleaseStop没有被声明为volatile，线程执行run的时候检查的是自己的副本，就不能及时得知其他线程已经调用tellMeToStop()修改了pleaseStop的值。

Volatile一般情况下并不能代替sychronized，因为volatile不能保证操作的原子性，即使只是i++，实际上也是由多个原子操作组成：read i; inc; write i，假如多个线程同时执行i++，依然可能由于不同线程交替执行而出现写入脏数据的情况。也就是说，如果对变量值的修改需要依赖于变量之前的值，那么volatile不能保证一致性，需要用sychronized，或者使用atomic类型(java.util.concurrent.atomic.*)；而上面的代码例子是可以使用volatile的典型场景。

Volatile的另外一个作用是禁止指令的重排序优化。在一般情况下，Java执行语句的顺序可能会因为自动优化而修改，例如下面的例子，initialized的赋值有可能在doInitialize()之前就执行了，线程B就有可能不会正确的等待初始化完成。

``` java
boolean initialized = false;
```

``` java
// run in one thread
doInitialize();
initialized = true;
```

``` java
// run in another thread
while (!initialized) {
  sleep();
}
```

如果将initialized声明为volatile，就能保证它的执行顺序不会被改变（但JavaSE 5之前的版本依然会有问题）。

以前在讲述Java与C++区别的时候，有个著名的例子是double-checked locking在Java中不能使用，就是执行顺序优化造成的问题。但自JavaSE 5起，配合volatile的使用是可以实现double-checked locking的。

Reference：

- [http://www.javamex.com/tutorials/synchronization_volatile.shtml](http://www.javamex.com/tutorials/synchronization_volatile.shtml)
- [http://www.ibm.com/developerworks/cn/java/j-jtp06197.html](http://www.ibm.com/developerworks/cn/java/j-jtp06197.html)
- [深入理解Java虚拟机](http://book.douban.com/subject/6522893/)
