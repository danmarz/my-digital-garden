---
Title: Explain the event loop in JavaScript
---

# Explain the event loop in JavaScript

The event loop is the foundation of how JavaScript executes code. You may or may not be asked this directly, but the concepts will be helpful regardless. Understanding the event loop will not only help for interviews, but also make you a better JS developer overall.

![](https://skilled.dev/images/eventloop.png)

The two terms you would want to mention are the "call stack" and the "event queue".

-   Call Stack: Where our main program runs synchronously
-   Event Queue: Where the callbacks of asynchronous code run once the stack is clear
-   Event Loop: Watches the stack. When it's empty, it grabs the first item in the event queue and passes it to the stack to run
-   Heap: Where we store our objects and variables in memory

You may have heard that JavaScript is "single-threaded non-blocking IO". When we say JavaScript is single-threaded, it means the main program runs in-order, line-by-line on a single thread, by adding items to the stack. However, when we say it's non-blocking IO, this means thats tasks which aren't part of our main program are actually pushed off to another thread to be handled concurrently. The result is passed to a callback and added to the event queue to handle once the stack is clear.

If you haven't watched this video by Philip Roberts, it's essential content for any JS developer.
<iframe width="560" height="315" src="https://www.youtube.com/embed/8aGhZQkoFbQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jake Archibald also gave an equally excellent talk on the topic.

<iframe width="560" height="315" src="https://www.youtube.com/embed/cCOL7MC4Pl0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

