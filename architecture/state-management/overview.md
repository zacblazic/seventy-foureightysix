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
