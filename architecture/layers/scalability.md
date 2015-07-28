
# Scaling MVC

On the web server, the .NET Framework maintains a pool of threads that are used to service ASP.NET requests. When a request arrives, a thread from the pool is dispatched to process that request. If the request is processed synchronously, the thread that processes the request is busy while the request is being processed, and that thread cannot service another request.

This might not be a problem, because the thread pool can be made large enough to accommodate many busy threads. However, the number of threads in the thread pool is limited (the default maximum for .NET 4.5 is 5,000). In large applications with high concurrency of  long-running requests, all available threads might be busy. This condition is known as thread starvation. When this condition is reached, the web server queues requests. If the request queue becomes full, the web server rejects requests with an HTTP 503 status (Server Too Busy). The CLR thread pool has limitations on new thread injections. If concurrency is bursty (that is, your web site can suddenly get a large number of requests) and  all available request threads are busy because of backend calls with  high latency, the limited thread injection rate can make your application respond very poorly.  Additionally, each new thread added to the thread pool has overhead (such as 1 MB of stack memory). A web application  using synchronous methods to service high latency calls where the thread pool grows to the .NET 4.5 default maximum  of 5, 000 threads would consume approximately 5 GB more memory than an application able the service the same requests using asynchronous methods and only 50 threads. When you’re doing asynchronous work, you’re not always using a thread. For example, when you make an asynchronous web service request, ASP.NET will not be using any threads between the async method call and the await.  Using the thread pool to service requests with high latency can lead to a large memory footprint and poor utilization of the server hardware.

## Asynchronous requests

In web applications that sees a large number of concurrent requests at start-up or has a bursty load (where concurrency increases suddenly), making these web service calls asynchronous will increase the responsiveness of your application. An asynchronous request takes the same amount of time to process as a synchronous request. For example, if a request makes a web service call that requires two seconds to complete, the request takes two seconds whether it is performed synchronously or asynchronously. However, during an asynchronous call, a thread is not blocked  from responding to other requests while it waits for the first request to complete. Therefore, asynchronous requests prevent request queuing and thread pool growth when there are many concurrent requests that invoke long-running operations.

## Choosing Synchronous or Asynchronous

In general, use synchronous methods for the following conditions:

* The operations are simple or short-running.
* Simplicity is more important than efficiency.
* The operations are primarily CPU operations instead of operations that involve extensive disk or network overhead. Using asynchronous action methods on CPU-bound operations provides no benefits and results in more overhead.

In general, use asynchronous methods for the following conditions:
* You're calling services that can be consumed through asynchronous methods, and you're using .NET 4.5 or higher.
* The operations are network-bound or I/O-bound instead of CPU-bound.
* Parallelism is more important than simplicity of code.
* You want to provide a mechanism that lets users cancel a long-running request.
* When the benefit of switching threads out weights the cost of the context switch. In general, you should make a method asynchronous if the synchronous method waits on the ASP.NET request thread while doing no work.  By making the call asynchronous,  the ASP.NET request thread is not stalled doing no work while it waits for the web service request to complete.
* Testing shows that the blocking operations are a bottleneck in site performance and that IIS can service more requests by using asynchronous methods for these blocking calls.

http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
