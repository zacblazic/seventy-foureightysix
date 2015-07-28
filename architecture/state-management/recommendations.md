# State Management Recommendations
ASP.NET provides multiple ways to maintain state between server round trips. Which of these options you choose depends heavily upon your application, and it should be based on the following criteria:

* How much information do you need to store?
* Does the client accept persistent or in-memory cookies?
* Do you want to store the information on the client or on the server?
* Is the information sensitive?
* What performance and bandwidth criteria do you have for your application?
* What are the capabilities of the browsers and devices that you are targeting?
* Do you need to store information per user?
* How long do you need to store the information?
* Do you have a Web farm (multiple servers), a Web garden (multiple processes on one machine), or a single process that serves the application?

## Client Side
These options typically have minimal security but fast server performance because the demand on server resources is modest. However, because you must send information to the client for it to be stored, there is a practical limit on how much information you can store this way.

### View State

#### Advantages
* No server resources are required. The view state is contained in a structure within the page code.
* Simple implementation. View state does not require any custom programming to use. It is on by default to maintain state data on controls.
* Enhanced security features. The values in view state are hashed, compressed, and encoded for Unicode implementations, which provides more security than using hidden fields.

#### Disadvantages
* Performance considerations. Because the view state is stored in the page itself, storing large values can cause the page to slow down when users display it and when they post it.
* Device limitations. Mobile devices might not have the memory capacity to store a large amount of view-state data.
* Potential security risks. Although view state stores data in a hashed format, it can still be tampered with.

### Control State
#### Advantages
* No server resources are required. By default, control state is stored in hidden fields on the page.
* Reliability. Because control state cannot be turned off like view state, control state is a more reliable method for managing the state of controls.
* Versatility. Custom adapters can be written to control how and where control-state data is stored.

#### Disadvantages
* Some programming is required.

## Hidden Fields
#### Advantages
* No server resources are required. The hidden field is stored and read from the page.
* Widespread support. Almost all browsers and client devices support forms with hidden fields.
* Simple implementation. Hidden fields are standard HTML controls that require no complex programming logic.

#### Disadvantages
* Potential security risks. The hidden field can be tampered with.
* Simple storage architecture. The hidden field does not support rich data types.
* Performance considerations. Because hidden fields are stored in the page itself, storing large values can cause the page to slow down.
* Storage limitations. If the amount of data in a hidden field becomes very large, some proxies and firewalls will prevent access to the page that contains them.

### Cookies
Cookies are useful for storing small amounts of frequently changed information on the client.

#### Advantages
* Configurable expiration rules.  The cookie can expire when the browser session ends, or it can exist indefinitely on the client computer, subject to the expiration rules on the client.
* No server resources are required. The cookie is stored on the client and read by the server after a post.
* Simplicity. The cookie is a lightweight, text-based structure with simple key-value pairs.
* Data persistence. Although the durability of the cookie on a client computer is subject to cookie expiration processes on the client and user intervention, cookies are generally the most durable form of data persistence on the client.

#### Disadvantages
* Size limitations. Most browsers place a 4096-byte limit on the size of a cookie, although support for 8192-byte cookies is becoming more common in newer browser and client-device versions.
* User-configured refusal. Some users disable their browser or client device's ability to receive cookies, thereby limiting this functionality.
* Potential security risks. Cookies are subject to tampering. Users can manipulate cookies on their computer, which can potentially cause a security risk or cause the application that is dependent on the cookie to fail. Also, although cookies are only accessible by the domain that sent them to the client, hackers have historically found ways to access cookies from other domains on a user's computer.

### Query strings
#### Advantages
* No server resources are required. The query string is contained in the HTTP request for a specific URL.
* Widespread support. Almost all browsers and client devices support using query strings to pass values.
* Simple implementation. ASP.NET provides full support for the query-string method, including methods of reading query strings using the Params property of the HttpRequest object.

#### Disadvantages
* Potential security risks. The information in the query string is directly visible to the user via the browser's user interface.
* Limited capacity. Some browsers and client devices impose a 2083-character limit on the length of URLs.

## Server Side
### Application State
Data that is shared by multiple sessions and does not change often is the ideal type of data to insert into application-state variables.

#### Advantages
* Simple implementation.
* Application scope. Because application state is accessible to all pages in an application, storing information in application state can mean keeping only a single copy of the information.

#### Disadvantages
* Application scope. The scope of application state can also be a disadvantage. Variables stored in application state are global only to the particular process the application is running in, and each application process can have different values. Therefore, you cannot rely on application state to store unique values or update global counters in Web-garden and Web-farm server configurations.
* Limited durability of data. Because global data that is stored in application state is volatile, it will be lost if the Web server process containing it is destroyed, such as from a server crash, upgrade, or shutdown.
* Resource requirements. Application state requires server memory, which can affect the performance of the server as well as the scalability of the application.

### Session State
#### Advantages
* Simple implementation.
* Session-specific events. Session management events can be raised and used by your application.
* Data persistence. Data placed in session-state variables can be preserved through restarts.
* Platform scalability. Session state can be used in both multi-computer and multi-process configurations, therefore optimizing scalability scenarios.
* Cookieless support. Session state works with browsers that do not support HTTP cookies, although session state is most commonly used with cookies to provide user identification facilities to a Web application. Using session state without cookies, however, requires that the session identifier be placed in the query string, which is subject to the security issues stated in the query string section of this topic.
* Extensibility. You can customize and extend session state by writing your own session-state provider.

#### Disadvantages
* Performance considerations. Session-state variables stay in memory until they are either removed or replaced, and therefore can degrade server performance.

### Profile Properties
#### Advantages

* Data persistence. Data placed in profile properties is preserved through restarts.
* Platform scalability. Profile properties can be used in both multi-computer and multi-process configurations, therefore optimizing scalability scenarios.
* Extensibility. In order to use profile properties, you must configure a profile provider.

#### Disadvantages
* Performance considerations. Profile properties are generally slower than using session state because instead of storing data in memory, the data is persisted to a data store.
* Additional configuration requirements.
* Data maintenance. Profile properties require a certain amount of maintenance. Because profile data is persisted to non-volatile storage, you must make sure that your application calls the appropriate cleanup mechanisms, which are provided by the profile provider, when data becomes stale.

### Database Support

Basically any website with a database.

#### Advantages

* Security. Access to databases requires rigorous authentication and authorization.
* Storage capacity. You can store as much information as you like in a database.
* Data persistence. Database information can be stored as long as you like, and it is not subject to the availability of the Web server.
* Robustness and data integrity. Databases include various facilities for maintaining good data, including triggers and referential integrity, transactions, and so on.
* Accessibility. The data stored in your database is accessible to a wide variety of information-processing tools.
* Widespread support. There is a large range of database tools available, and many custom configurations are available.

#### Disadvantages

* Complexity. Using a database to support state management requires that the hardware and software configurations be more complex.
* Performance considerations. Poor construction of the relational data model can lead to scalability issues.
