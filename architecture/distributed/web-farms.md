
# Web Farms

Enables you to automate the installation and configuration of platform components across the server farm, and enables you to automatically synchronize and deploy ASP.NET applications across them.

It also supports integration with load balancers - and enables you to automate updates across your servers (it can automatically pull servers one-at-a-time out of the load balancer rotation, update them, and then inject them back into rotation).

## Using Web Farms

The Microsoft Web Farm Framework enables you to easily define a “Server Farm” that you can add any number of servers into.  Servers participating in the “Server Farm” will then be automatically updated, provisioned and managed by the Web Farm Framework.

What this means is that you can install IIS (including modules like UrlRewrite, Media Services, etc), ASP.NET, and custom SSL certificates once on a primary server – and then the Web Farm Framework will automatically replicate and provision the exact same configuration across all of the other web servers in the farm (no manual or additional steps required).

You can then create and configure an IIS Application Pool and a new Site and Application once on a primary server – and the Web Farm Framework will automatically replicate and provision the settings to all of the other web servers in the farm.  You can then copy/deploy an ASP.NET application once on the primary server – and the Web Farm Framework will automatically replicate and provision the changes to all of the web servers in the farm (no manual or additional steps required).

The Web Farm Framework eliminates the need to manually install/manage things across a cluster of machines.  It handles all of the provisioning and deployment for you in a completely automated way.

## Load Balancing

* Can integrate with an HTTP load balancer so that as web servers in the farm are updated with changes, they can be automatically pulled out of a load balancer rotation, updated, and then added back in.

* Built-in support for the IIS Application Request Routing (ARR) service (which supports automatic load balancing of HTTP requests across multiple machines in a web-farm)

## Setting Up & Provisioning a Web Farm

Let’s walkthrough a simple scenario where we want to setup and manage a server farm made up of two web servers.  We’ll call these two web-servers the “DemoPrimary” and “DemoSeconday” servers.  We’ll use a third “DemoController” machine to coordinate and manage the web farm.

* Install the framework (controller)
* Create new "Server Farm"
* Indicate we want to use load balancing & provision servers
* Next, add servers to the farm (can do this later too)
* Add first server, selecting that it should be the primary (which other servers will replicate)
* Add second server
* WFF will connect to the servers and provision management software to target machines
* Farm configuration is complete

If we were to create sites or application pools on the “DemoPrimary” web server, or install or update applications on it, they would be automatically replicated/synchronized on the “DemoSeconday” server.

The Web Farm Framework will automate keeping them synchronized with our Primary Server – and enable us to manage the entire farm of servers as a group.

## Managing a Web Farm

Clicking on the “DemoFarm” sub-node within the IIS Admin Tool enables us to manage, track and configure our server farm.

The view above combines the settings of both the Web Farm Framework, as well as the Application Request Routing capability of IIS (which adds load-balancing and cache management support).

You can click on the “Servers” sub-node of “DemoFarm” to see how the servers within the web farm are doing, if they are currently performing any deployment/provisioning operations, and if any errors have occurred on them.  If any provisioning/deployment errors have occurred anywhere in the farm, you can drill down to detailed tracing to see what they were.

### Platform Provisioning

The Web Farm Framework enables you to easily add more platform components to your server-farm machines at anytime by using the “Platform Provisioning” icon above.  This integrates our Microsoft Web Platform Installer technology, and enables you to easily install platform components on all of your web farm machines.  It also allows you to easily check for any version differences in platform components between your primary and secondary machines in the farm.

### Application Provisioning

The Web Farm Framework enables you to easily deploy and replicate/synchronize Sites, Applications, Content and Settings across a Web Farm.  It uses the Microsoft Web Deploy technology to enable application deployment in an automated fashion (no manual steps required for both adding new applications and updating existing ones).

By default, the Web Farm Framework will automatically synchronize the Sites, Applications, Content and Settings we’ve configured on our “DemoPrimary” server to our “DemoSeconday” server (and any other web-servers we later add to the server farm).

http://weblogs.asp.net/scottgu/introducing-the-microsoft-web-farm-framework

http://www.iis.net/learn/web-hosting/microsoft-web-farm-framework-20-for-iis-7/overview-of-the-web-farm-framework-20-for-iis

http://dotnetcodr.com/2013/06/17/web-farms-in-net-and-iis-part-1-a-general-introduction/
