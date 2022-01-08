---
Title: What is CORS
---

# What is CORS

CORS stands for "cross-origin resource sharing" and is a mechanism allowing us to access resources from other domains (origins). It specifically refers to how a client connects to servers. CORS determines how a browser is able to utilize files/data from domains outside a user's current URL. Because a website loads resources from all around the web, the `same-origin` is used to mitigate risk and prevent hijacking a user's visit.

For example, if you are on `facebook.com`, you are able to download all the HTML, CSS, JavaScript, font, and image files from them. In addition, the browser won't block you from accessing an API like `facebook.com/api/users`. However, if you try to access data from a domain outside of Facebook while still on the site, such as calling the Twitter API `twitter.com/api`, you may or may not be able to fetch this data depending on how CORS is configured.

![](https://skilled.dev/images/cors.png)

Because web applications have become so complex and dynamic, an understanding of CORS is essential and typically dictates the functionality of our clients. Running into a case where CORS is not enabled is a common issue that needs to be handled.

CORS is determined by the servers that provide the files/data. They use the different `Access-Control-*` headers to dictate how their data can be accessed in browsers.

The `same-origin` policy only applies to the client. If there is a resource you need where CORS isn't enabled, you can access it using your server and then respond from your server to the client.

# ---

Tags: #coding #interview

Topics: [[javascript]] 


