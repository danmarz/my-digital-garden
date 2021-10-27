---
Title: Microtask queue
---

## The Microtask queue in [[javascript]]

The microtask queue is a function queue in JS that sits between the *call stack* and its underlying *global()* context ― and the *callback queue* (also called the *task queue*) on which *OnCompletion* functions are queued from the [[web browser APIs]] to be put on the *call stack* by the [[event loop]].

The microtask queue is where [[promises]] queue their *OnFulfillment:* functions (which are passed to the promise object hidden array as *.this* functions).

The Event Loop priorities are:
1. Call stack & *global() context* synchronous code =>
2. Microtask queue (promise functions onFulfilled) =>
3. Callback queue (Task queue - Web APIs onCompletion)