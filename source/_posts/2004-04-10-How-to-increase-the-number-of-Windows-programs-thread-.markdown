---
layout: post
title: "如何增加windows程序的线程数量?"
comments: true
date: 2004-04-10 06:10
tags:
- SoftwareDev
---
编写测试程序, 发现超过1000个线程, 程序执行就会出现内存错误. 原来我还以为是windows对线程数量有限制, 查资料才知道支持线程多少并不是操作系统决定的, 而是与程序的堆栈空间大小有关.

在VC中, 修改Project Setting - Link - Stack allocation - Reserve, 缺省为1M(0x100000), 将这个数值降低, 也就是减少每个线程的栈空间大小, 就可以增加线程数量. 这是因为用户地址空间总共是2GB, 线程保留栈空间大小乘以线程数量不能大于2GB.

因为上下文切换的开销, 线程过多效率会降低, 我做这个测试程序就是想知道这个效率曲线到底是怎样的.

以下为网上找到的资料:

I just found this under CreateThread() in MSDN:  
  
The number of threads a process can create is limited by the available virtual memory. By default, every thread has one megabyte of stack space. Therefore, you can create at most 2028 threads. If you reduce the default stack size, you can create more threads. However, your application will have better performance if you create one thread per processor and build queues of requests for which the application maintains the context information. A thread would process all requests in a queue before processing requests in the next queue.  
  
So the number of threads you can create is capped by the 2 GB user address space available to your process.  
  
So you have a few choices, as I see it:  
  
1) Run multiple copies of your server at once and coordinate between them. You will need endless amounts of swap space.  
2) Reduce the amount of stack used per thread (lowering from 1 Mb to 100Kb should get you 40,000 threads).  
3) Re-architect your app to not require one thread per connection. Have each thread monitor many connections using (for example) a select() call.
