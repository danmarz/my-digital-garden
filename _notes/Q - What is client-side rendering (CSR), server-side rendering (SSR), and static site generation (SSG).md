---
Title: What is client-side rendering (CSR), server-side rendering (SSR), and static site generation (SSG)
---

# What is client-side rendering (CSR), server-side rendering (SSR), and static site generation (SSG)

-   **Client-side rendering**: CSR means we render our applications entirely in the browser. When a page is first loaded, an empty HTML document is passed to the client with a `<script>` tag that retrieves JavaScript. Once this JavaScript loads, it builds the HTML for the page. CSR can be great for fast connections, but can produce a white stagnant screen if a connection is slow. CSR has been criticized for being bad for SEO because it requires the web crawler to be able to effectively run the JavaScript to build the page.
-   **Server-side rendering**: When the user requests a URL, the server generates the HTML for the first page load, and all the JS and CSS files are passed down as well. Once on the client, the additional pages behave as a single page application. This is better for slow connections because the initial work is done on the server and provides an initial page without needing to download and execute JavaScript files. This also helps with SEO because the web crawler receives a full page at the initial request.
-   **Static site generation**: Pages of a website are built as static HTML files before deploying. This is the fastest of the three methods because the files are pre-built and can be cached in a CDN which means there is no wait time for a server to render them, and they can be sent to the client very quickly. Once the initial HTML is loaded, it is passed the JS and CSS files and often functions as a single page application after this.
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

