---
layout: post
title: "Understanding WebLogic WorkManager"
date: 2012-08-10 22:54
comments: true
tags:
- AppServer
- WebLogic
---
Since WebLogic Server 9.0, new concepts for workload management is introduced. WorkManager replaces execute queue as defined in earlier releases.

All WorkManagers share a common thread pool and a priority-based queue. The size of the thread pool is determined automatically by the kernel and resized as needed.  Priority of the requests is dynamic and computed internally to meet the stated goals. 

The thread pool is so-called self-tunning. It monitors the overall throughput every two seconds and uses the collected data to determine if thread count needs to change. Present thread count, the measured throughput, and the past history is taken into account by the algorithm to determine if the thread count needs to increase or decrease, and new threads are automatically added to the pool or removed, as needed.

It's no longer have to (and is unable to) configure thread counts on a WorkManager nor the common thread pool. However, it's possible to affect how server prioritizes work and allocates threads by parameters defined in WorkManager. Each WorkManager can contain following types of components:

+ Request class 
    - Fair-share
    - Response-time goal
    - Context based
+ Constraints
    - Minimum threads constraint
    - Maximum threads constraint
    - Capacity

Request class affects how requests are prioritized. Fair-share request class changes thread usage weight of a WorkManage, which is by default 50. Response-time goal request class specifies the response-time goal in milliseconds. Context-based request class is a compound request class that provides a mapping between the request context and the above two request classes. With context-based request class, it's possible to specify different request classes for the same servlet invocation depending on the user associated with the invocation.

A constraint defines minimum and maximum numbers of threads allocated to execute requests and the total number of requests that can be queued or executing before WebLogic Server begins rejecting requests. Constraint is referenced in WorkManager by constraint name. So a constraint can be shared by several WorkManagers and set the common limit.

Minimum threads constraint makes sure that during periods of high workloads, there would still be a certain number of threads from the self-tuning thread pool available to process work requests for all work managers that reference the minimum threads constraint. Using the minimum threads constraint could result in some side-effects. Users should consider adding a minimum threads constraint to a work manager configuration when it is critical that progress must be made for an application even when the WLS server is under very heavy load, such as work that would have resulted in server-to-server deadlock if not being processed promptly. It is not to be used as a mean of prioritizing workload among different work managers. 

The maximum threads constraint limits the number of threads in the self-tuning thread pool that can be used for executing work for all work managers that references the same constraint. The maximum threads constraint is not designed as a mean to prioritize workloads among different work managers. It is most useful when there are other known limitations where a hard upper limit should be put on the number of threads that should be assigned for processing work requests, and that allocating more threads for processing the workload would not increase the overall throughput.

A typical use case of maximum threads constraint is to take a data source connection pool size as the max constraint.

    <max-threads-constraint>
      <name>MyConstraint</name>
      <pool-name>MyDataSource</pool-name>
    </max-threads-constraint>

The capacity constraint defines the maximum number of requests that can be queued (waiting for thread) or are executing. Overload action, e.g. refuse with HTTP response code 503, is taken after the capacity is reached. 

I draw a picture to show the threadpool and queue. Requests dispatched with different WorkManager are in different color.

![](/attachments/2012/8/workmanager.jpg)

- Self-tuning thread pool thread allocation ratio amoung WorkManagers is affected by request class. 
- Minimum threads constraint limits: never queue request, unless `executing >= min-threads-constraint.count`
- Maximum threads constraint limits: `executing <= max-threads-constraint.count`
- capacity contraint limits: `executing + queued <= capacity.count`

WorkManager can be created for a specific application or module (ejb/war), which is called application-scoped WorkManager. If there is no special requirement on individual application or module, you can create global Work Managers that are available to all applications and modules. An application uses a globally-defined Work Manager as a template. Each application creates its own instance, this means that each application has its own default WorkManager that is not shared with other applications. WebLogic Server provides a built-in default Work Manager which may be sufficient for most application requirements.

Reference:

- [Workload Management in WebLogic Server 9.0](http://www.oracle.com/technetwork/articles/entarch/workload-management-088692.html)
- [Threads Constraints in Work Managers](https://blogs.oracle.com/WebLogicServer/entry/threads_constraints_in_work_ma)
- [Using Work Managers to Optimize Scheduled Work](http://docs.oracle.com/cd/E15051_01/wls/docs103/config_wls/self_tuned.html)
