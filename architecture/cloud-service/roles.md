# Roles

An application designed to be a cloud service in Windows Azure consists of different computational resources that collectively process information and interact with each other and the external world. A cloud service in Windows Azure can contain up to five of the following role definitions:

* Web Role – A service component that is customized for web application programming as supported by Internet Information Services (IIS) 7 and ASP.NET. The web role is intended to serve as the front-end to your cloud service.

* Worker Role – A service component that is useful for generalized development, and may perform background processing for a web role. A worker role is frequently used for long-running tasks that are non-interactive, but you can host any type of workload.

## Using Roles To Create an Application

In Windows Azure, the web role contains websites or other code that are supported by Internet Information Services (IIS).

The primary difference between a web role and a worker role is support for IIS.

Some of the differences are:

* The web role provides support for presenting a user facing frontend through IIS. While you can write code to present a user interface using a worker role, the web role makes creating this type of application easier.
* In the web role, IIS handles the efficiency of threading. In the worker role you must handle threading issues yourself.
* You must provide a run method that is called to initiate processing.
* The security perimeter is different between the web and worker role. In the web role the access control lists (ACLs) for certificates are set to support IIS and network services by default. When using a worker role you must set the ACLs to support the permissions your application requires.

The types of tasks a worker role is typically used are:
* Long running tasks that are asynchronous that the user does not have to wait for.
* To host application services that do not require a user interface.
* Background services listening to a queue.
* Running TCP based services.
* Compute intensive jobs.

## Role Lifecycle

The RoleEntryPoint class includes methods that are called by Windows Azure when it starts, runs, or stops a web or worker role.

A worker role must extend the RoleEntryPoint class. For web roles, extending RoleEntryPoint is optional.

* The OnStart and OnStop methods return a boolean value, so it is possible to return false from these methods. Returning false terminates the role process.
* Any uncaught exception within an overload of a RoleEntryPoint method is treated as an unhandled exception, terminating the role process.

### OnStart Method

The OnStart method is called when your role instance is brought online by Windows Azure. While the OnStart code is executing, the role instance is marked as Busy and no external traffic will be directed to it by the load balancer. You can override this method to perform initialization work, such as implementing event handlers and starting Windows Azure Diagnostics. For more information about diagnostics, see Collect Logging Data by Using Azure Diagnostics.

If OnStart returns true, the instance is successfully initialized and Windows Azure calls the RoleEntryPoint.Run method. If OnStart returns false, the role terminates immediately, without executing any planned shutdown sequences.

### OnStop Method

The OnStop method is called after a role instance has been taken offline by Windows Azure and before the process exits. You can override this method to call code required for your role instance to cleanly shut down.

Code running in the OnStop method has a limited time to finish when it is called for reasons other than a user-initiated shutdown. After this time elapses, the process is terminated, so you must make sure that code in the OnStop method can run quickly or tolerates not running to completion. The OnStop method is called after the Stopping event is raised.

### Run Method

You can override the Run method to implement a long-running thread for your role instance.

If you do override the Run method, your code should block indefinitely. If the Run method returns, the role is automatically gracefully recycled.

## Combination of Roles

One of the most common application patterns in Windows Azure is for a web role to receive incoming requests and then use Windows Azure queues to pass them to instances of a worker role to process. The worker role periodically looks in the queue for messages to see if there is any work to do. If there is, it performs the task. The web role typically retrieves completed work from persistent storage, such as a blob or a table. The following diagram shows this typical design pattern.

https://msdn.microsoft.com/en-us/library/azure/hh180152.aspx
