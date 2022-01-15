---
Title: Explain AJAX
---

# Explain AJAX

AJAX stands for "asynchronous JavaScript and XML" and is a core feature of modern web development. It allows us to create fast, dynamic, and interactive web applications.

AJAX is how we make API requests from the client to send and receive data within a page.

In the client/browser, instead of needing to transition pages, reload the current page, or submit a form to change the state, we can make HTTP requests asynchronously to send and receive data between the client and a server. This allows us to decouple the presentation layer from the data layer to create powerful web applications. We can update parts of the page by exchanging smaller amounts of data with the server without needing to rebuild the entire page.

When we talk about making an API request on the client, it is doing so through AJAX. JSON has replaced XML as the primary data exchange format and has become the standard. When AJAX was first introduced, the `XMLHttpRequest` pattern was used to facilitate AJAX requests, but now the more modern method is to use `fetch` or some similar promise-based library.
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

