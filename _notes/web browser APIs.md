---
Title: Web Browser APIs
---

## JS & Web Browser APIs

The web browser is a composite piece of software which runs [[javascript]] (we typically do not run a Node.JS server at home 😊) **PLUS** it is also tasked with opening and closing sockets, making requests and displaying the website on our screen (rendering HTML and applying CSS) among others:

|Web browser features | JS |
|---      | ---|
|dev tools | |
|console =>| **console**|
| sockets ||
| making networking requests => | **fetch** (old xhr)|
| HTML DOM (Document Object Model) => |**document**.|
| timer => | **setTimeout**|
| localStorage => |**localStorage**|

In JS we have a bunch of **_façade functions_**(functions that *look like they're JS*, but are actually *fronts* for Web Browser features) which interface with the browser's API. 

These functions delegate the *timer* and the actual JS function to run to the Web Browser and consider their job complete. It's then the browser's task to create the timer, monitor the time passed and return the function to JS's *callback queue* when it's finished. JS [[event loop]] handles the function execution from there.

