# State Management Overview

* View state
* Control state
* Hidden fields
* Cookies
* Query strings
* Application state
* Session state
* Profile Properties

View state, control state, hidden fields, cookies, and query strings all involve storing data on the client in various ways. However, application state, session state, and profile properties all store data in memory on the server. Each option has distinct advantages and disadvantages, depending on the scenario.

## Client State Management

### View State

The ViewState property provides a dictionary object for retaining values between multiple requests for the same page. This is the default method that the page uses to preserve page and control property values between round trips.

When the page is processed, the current state of the page and controls is hashed into a string and saved in the page as a hidden field.

### Control State

Sometimes you need to store control-state data in order for a control to work properly. For example, if you have written a custom control that has different tabs that show different information, in order for that control to work as expected, the control needs to know which tab is selected between round trips. The ViewState property can be used for this purpose, but view state can be turned off at a page level by developers, effectively breaking your control. To solve this, the ASP.NET page framework exposes a feature in ASP.NET called control state.

The ControlState property allows you to persist property information that is specific to a control and cannot be turned off like the ViewState property.

### Hidden Fields

ASP.NET allows you to store information in a HiddenField control, which renders as a standard HTML hidden field. A hidden field does not render visibly in the browser, but you can set its properties just as you can with a standard control. When a page is submitted to the server, the content of a hidden field is sent in the HTTP form collection along with the values of other controls. A hidden field acts as a repository for any page-specific information that you want to store directly in the page.

It is easy for a malicious user to see and modify the contents of a hidden field. Do not store any information in a hidden field that is sensitive or that your application relies on to work properly.

### Cookies

A cookie is a small amount of data that is stored either in a text file on the client file system or in-memory in the client browser session. It contains site-specific information that the server sends to the client along with page output. Cookies can be temporary (with specific expiration times and dates) or persistent.

You can use cookies to store information about a particular client, session, or application. The cookies are saved on the client device, and when the browser requests a page, the client sends the information in the cookie along with the request information. The server can read the cookie and extract its value. A typical use is to store a token (perhaps encrypted) indicating that the user has already been authenticated in your application.

The browser can only send the data back to the server that originally created the cookie. However, malicious users have ways to access cookies and read their contents. It is recommended that you do not store sensitive information, such as a user name or password, in a cookie. Instead, store a token in the cookie that identifies the user, and then use the token to look up the sensitive information on the server.

### Query String

A query string is information that is appended to the end of a page URL.

Query strings provide a simple but limited way to maintain state information. For example, they are an easy way to pass information from one page to another, such as passing a product number from one page to another page where it will be processed. However, some browsers and client devices impose a 2083-character limit on the length of the URL.

## Server State Management

ASP.NET offers you a variety of ways to maintain state information on the server, rather than persisting information on the client.

### Application State

ASP.NET allows you to save values using application state — which is an instance of the HttpApplicationState class — for each active Web application. Application state is a global storage mechanism that is accessible from all pages in the Web application.

Application state is stored in a key/value dictionary that is created during each request to a specific URL. You can add your application-specific information to this structure to store it between page requests.

### Session State

ASP.NET allows you to save values by using session state — which is an instance of the HttpSessionState class — for each active Web-application session.

Session state is similar to application state, except that it is scoped to the current browser session. If different users are using your application, each user session will have a different session state.

Session state is structured as a key/value dictionary for storing session-specific information that needs to be maintained between server round trips and between requests for pages.

You can use session state to accomplish the following tasks:
* Uniquely identify browser or client-device requests and map them to an individual session instance on the server.
* Store session-specific data on the server for use across multiple browser or client-device requests within the same session.
* Raise appropriate session management events. In addition, you can write application code leveraging these events.

### Profile Properties

SP.NET provides a feature called profile properties, which allows you to store user-specific data. This feature is similar to session state, except that the profile data is not lost when a user's session expires. The profile-properties feature uses an ASP.NET profile, which is stored in a persistent format and associated with an individual user. The ASP.NET profile allows you to easily manage user information without requiring you to create and maintain your own database. In addition, the profile makes the user information available using a strongly typed API that you can access from anywhere in your application. You can store objects of any type in the profile. The ASP.NET profile feature provides a generic storage system that allows you to define and maintain almost any kind of data while still making the data available in a type-safe manner.

To use profile properties, you must configure a profile provider. ASP.NET includes a SqlProfileProvider class that allows you to store profile data in a SQL database, but you can also create your own profile provider class that stores profile data in a custom format and to a custom storage mechanism such as an XML file, or even to a web service.

Because data that is placed in profile properties is not stored in application memory, it is preserved through Internet Information Services (IIS) restarts and worker-process restarts without losing data.
