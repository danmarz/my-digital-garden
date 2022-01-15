---
Title: What is a single page app and some of its pros and cons
---

# What is a single page app and some of its pros and cons

Before the modern internet, websites were multi-page, and each page would load independently. When you clicked a link, you would navigate to this new page and the server would send its HTML, and your browser would need to download the JS and CSS required for displaying that page.

Single page applications interact with the browser to dynamically rewrite a page's HTML instead of reloading the page and downloading new HTML from a server. It instead requests only the data needed to populate a new URL.

The name "single page" means that the page is only loaded once, and subsequently receives all the JavaScript and CSS code which allows it to build UI entirely on the client. This can offer big performance improvements because it only needs data to load new pages instead of loading an entirely new HTML document. It will also have all the necessary JS and CSS and won't need to load those each time (or will only need to download small incremental chunks to render a new URL).

Not only does it improve performance on the client, but it also reduces the load on the server. The backend becomes more focused on simply being an API and passing data as JSON to the frontend.

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]
