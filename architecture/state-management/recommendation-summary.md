# Client State

| State management option | Recommended usage |
| ----------------------- | ----------------- |
| View State | Use when you need to store small amounts of information for a page that will post back to itself. Using the ViewState property provides functionality with basic security. |
| Control state | Use when you need to store small amounts of state information for a control between round trips to the server. |
| Hidden fields | Use when you need to store small amounts of information for a page that will post back to itself or to another page, and when security is not an issue. |
| Cookies | Use when you need to store small amounts of information on the client and security is not an issue. |
| Query string | Use when you are transferring small amounts of information from one page to another and security is not an issue. |

# Server State

| State management option | Recommended usage |
| ----------------------- | ----------------- |
| Application state | Use when you are storing infrequently changed, global information that is used by many users, and security is not an issue. Do not store large quantities of information in application state. |
| Session state | Use when you are storing short-lived information that is specific to an individual session and security is an issue. Do not store large quantities of information in session state. Be aware that a session-state object will be created and maintained for the lifetime of every session in your application. In applications hosting many users, this can occupy significant server resources and affect scalability. |
| Profile properties | Use when you are storing user-specific information that needs to be persisted after the user session is expired and needs to be retrieved again on subsequent visits to your application. |
| Database support | Use when you are storing large amounts of information, managing transactions, or the information must survive application and session restarts. Data mining is a concern, and security is an issue. |
