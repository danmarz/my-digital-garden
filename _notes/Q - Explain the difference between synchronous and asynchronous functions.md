---
Title: Explain the difference between synchronous and asynchronous functions
---

# Explain the difference between synchronous and asynchronous functions

A JavaScript program executes exactly in-order on a single main thread. This means that code will run line-by-line synchronously. When our program encounters a synchronous function, it will add it to the call stack and the functions at the top of the call stack will run first. If a function takes a long time to finish, the following function must wait for it to complete before it can execute. This means long synchronous functions can cause our app to pause and appear frozen if not handled correctly.

JavaScript also has the concept of asynchronous functions. This is code that is handled off the main thread, and then the result is passed to a callback or a resolve/rejected `Promise`. This callback or `Promise` handler is passed to the event queue, and once the call stack is empty, the event loop will begin passing items from the event queue to the call stack. Each callback will then be executed in order as it is popped off the stack, and the async function response is passed as its argument. Asynchronous functions prevent the main thread from being blocked and allow us to perform expensive operations in a separate thread, or to wait for the result of a side effect and continue execution of the main program.

Examples of common asynchronous functions include API calls, database queries, file system I/O, and waiting for the resolution of a `setTimeout` call.

We can't discuss asynchronous JavaScript without going deeper into the `Promise` object and the `async`/`await` syntax. A `Promise` is the most common way to handle the response from an asynchronous function, and declaring a function as `async` means that JavaScript will just return a `Promise` from that function automatically. When we have an asynchronous function that returns a `Promise`, we can move past it and continue execution of the code on the main thread without blocking our program, and using the `then` method or `await` keyword allows us to handle the result of an asynchronous function after in completes.
# ---

Tags: #coding #interview

Topics: [[javascript]] 

