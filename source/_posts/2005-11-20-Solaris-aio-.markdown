---
layout: post
title: "Solaris上使用aio的问题"
comments: true
date: 2005-11-20 02:17
tags:
- SoftwareDev
---
我们程序的IRI backend使用了ACE Proactor作为TCP client的框架，proactor实质上是使用了asynchronous IO(aio)从socket上读写数据。异步IO将速度慢的I/O过程留在操作系统内部进行，不会阻塞用户线程，因此在应用中无需使用多线程也可以达到较高的效率。AIO在windows平台上使用比较广泛，称为CPIO。Solaris 9是支持aio的，按理说不会有什么问题，可是我们的程序就出现了很多奇怪的问题。

  * 有时CPU占用莫名其妙的很高，即使没有从网络收发数据 
  * 进程的lwp数量会变化，某些时候不断增加 
  * 用truss跟踪程序的时候，CPU就会降低

反复检查过程序，都没有发现问题，程序只有一个线程进入了proactor，怀疑是ACE的proactor内部创建了线程，但看过ACE代码，里面也没有spawn线程。而且aio就是为了提高用户线程效率，让单线程可以处理大量并发IO的，没有理由需要再创建多线程。

我们的鬼佬chief architect是hacker出身的，他跟我们一起查找问题，最终找到了原因。

>Some notes for us on AIO to help us better understand:
>
[1] [http://sunsite.uakom.sk/sunworldonline/swol-03-1996/swol-03-aio.html](http://sunsite.uakom.sk/sunworldonline/swol-03-1996/swol-03-aio.html)
>
[2] Keep in mind that any major improvement over standard read/write system calls requires the use of multi-processor machines, since AIOs are executed via light weight processes (LWPs). 
>
[3] aiorandom syiorandom  
Both programs read 262,144 data blocks. Each block is 4,096 bytes. aiorandom submits a batch of 30 asynchronous reads at a time. Whenever the number of outstanding AIO operations falls below 15, it submits another batch. Results were recorded from the command timex. 
>
&nbsp; | aiorandom | syiorandom  
---|---:|---:
real |42:43.92 |1:02:48.68  
user | 5:35.57  |  4:39.38  
sys  |5:24.02  |  2:41.23
>
For aiorandom, this is a speed-up of close to 30 percent in real (elapsed) time. Note how aiorandom consumed more user CPU time due to the extra work associated with managing the AIO-related data structures. System CPU time doubled thanks to aiorandom running multiple LWPs. Keep this in mind on busy, multi-user systems since heavy AIO applications will depress the performance of other apps. 
>
[4]  However, libaio is different; it implements system functions. Depending on the numbers of submitted AIO requests, libaio creates so-called "workers" (as implemented as LWP's) to process the IO requests. 
>
THIS IS WHAT IS KAIO .. [next line]
>
[1]  Solaris 2.5 implements kernel-supported asynchronous I/O. The kernel has implemented special, efficient AIO assists for the libaio. 
>
[2] [http://developers.sun.com/solaris/articles/thread_pools.html](http://developers.sun.com/solaris/articles/thread_pools.html)
>
Here is the code from aio.so:
>
[aio.c](http://cvs.opensolaris.org/source/xref/on/usr/src/uts/common/os/aio.c)
>
    394 /*  
    395  * wake up LWPs in this process that are sleeping in  
    396  * aiowait().  
    397  */  
    398 static int  
    399 aionotify(void)  
    400 {  
    401  aio_t *aiop;  
    402   
    403  aiop = curproc->p_aio;  
    404  if (aiop == NULL)  
    405   return (0);  
    406   
    407  mutex_enter(&aiop->aio_mutex);  
    408  aiop->aio_notifycnt++;  
    409  **cv_broadcast(&aiop->aio_waitcv);**  
    410  mutex_exit(&aiop->aio_mutex);  
    411   
    412  return (0);  
    413 }
>
>
If the application uses a third party library that implements asynchronous operations, this may not be the best solution. This is because of the "anonymous" nature of aiowait(), which returns the result of any asynchronous read or write operation. And if a third party AIO library is used in the application, this means that aiowait() could return result sets of the third party library AIO calls as well. Due to obvious interface disparities, this could lead to unpredictable behavior. 
>
.. i find out some very interesting things about the aio ... especially  
the CPU usage being high is very normal .. good if you want  
to make a high capacity/performance server that will run on   
its own on a SMP machine .. then aio gives a very strong   
efficiency and raw performance .. but use extra CPU because  
the process have very high granularity of checking all async   
operations .. much higher than a user space thread .. 
>
however this kind of server will hog CPU and if used in a mixed  
environment with many other systems on same host will not be  
a good solution .. also such process have no ability to define   
filters on the async events they listen for .. they will always get  
interrupts from any kind of asynch event in the system . .then   
have to go through that array of 1024 pointers and check if any  
of the non-null pointers in there will contain an event for them ....   
(thats why if you connect truss -p the CPU goes down .. all sigs  
are blocked for a process inside truss) 
>
the behaviour with CPU etc. that we see in ismapserver seems  
quite 'normal' for the aio .. from what i can read the last days and  
from what you can see in the solaris os code. (you can also look   
in kernel.c and condvar.c) 

看来aio并不是那么好用，对于我们的程序而言，IRI接口的网络吞吐量并不是瓶颈，而且我们的程序本来就是多线程程序，其实并不需要使用aio。程序中的其他网络接口使用的TP Reactor（基于多线程，select）就工作得很好，效率也远超出需求了。对于IRI backend，甚至用一个线程收一个线程发就够了，因为不需要处理太多connection，线程数量在这里不是问题。

还是应该遵循用最简单的方法来解决问题的原则。
