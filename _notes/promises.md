---
Title: JS Promises
---

## [[javascript]] promises

Promises are the most significant ES6 (EcmaScript 2016) feature that makes dynamic web applications possible by means of *Asynchronicity*. 

JS is:
- Single threaded (one command runs at a time)
- Synchronously executed (each line runs in order that the code appears)

But this means *slow functions block further code from running * i.e:
```js
const tweets = getTweets("httpe://twitter.com/will/1")
// 🛑 350ms wait while a request is sent to Twitter HQ servers

displayTweets(tweets)

// more code to run
console.log("I want to runnnn!")
```

### JS is not enough - we need new pieces (some of which arent't JavaScript at all)

At the core, JS has:
- Thread of execution
- Memory/variable environment
- Call stack

JS has no display renderer, no ability to manage sockets, no way to make calls to get resources, *it hasn't even got a timer*,these must come from external sources.. Like *setTimeout* function:

```js
function printHello(){ console.log("Hello"); }
setTimeout(printHello, 1000);
console.log("Me first!");
```
Which results in:
```
Me first!
Hello
```
Because the *setTimeout(function, timer\_in\_miliseconds)* DOESN'T belong to JS, it is delegated to the browser's API, therefore it is a **_façade function_** for which JS does not wait to complete as it does not run in JS thread of execution.

Other components external to JS are:
- [[web browser APIs|Web Browser APIs]]/Node background APIs
- [[promises|Promises]]
- [[event loop|Event Loop]], [[callbacks|Callback]]/*Task queue and micro task queue*

### Now we're interacting with a world outside of JS - we need rules now

A function delegated to [[web browser APIs]] when complete (setTimeout) is put in a queue called **Callback Queue** and it has to wait for *all* synchronous code that JS exec thread has next in line *before being allowed back into the JS Call Stack*. (can be 1 million console.log(), it doesn't matter).

```js
function printHello() { console.log('Hello') }
setTimeout(printHello, 0);

for (let i = 0; i < 10000; i++) {
 console.log('Me first!');
}
```

The problem with these *façade* callback functions is that **the response data is only available in the callback function** ( because [[closure]] ) therefore we have to handle EVERYTHING inside ONE function (multiple levels deep) => *callback hell*.

This is the way we *used* to use browser APIs, and it kind of still is under-the-hood although nowdays we use [[promises]] and JS [[async await]] much friendly syntax.

## Promises are 2-pronged 'façade' functions that both:

- Initiate background web browser work and
- Return a placeholder object (promise) immediately in [[javascript]] memory which has two built in properties *value: ....* which is an empty property for now (it's where the fetch request will store the data it recieves over the internet (as JSON?)) and an empty array called *onFulfillment: \[ \]* which is a **HIDDED** property which you can only access (append) with *.then*. 
  Also returned is ANOTHER hidden array called *onRejection: \[ \]* which will run in case ANY function returns an error (instead of returning *onFulfillment:* ). A function can be passed to this hidden array in two ways:
  1. using *.catch( )* <- after *.then( )*
  2. passing the error catching function as the second parameter to the `futureData.then(resolve, REJECT)` function. resolve = *onFulfillment*, reject = *onRejection*.

Both the web API and the JS exec context are intimately linked via the placeholder promise object, and unlike with [[callbacks]], there isn't any function passed as to be run *onCompletion*, instead any functions are to be *.then* executed on the promise object (when the request is fulfilled). i.e. `futureData.then(display);` this .then saves the display(data) function inside the hidden array called onFulfillment on the promise object and when the request is completed it is automatically passed the request data as argument and *automatically* added to the [[Microtask queue]] to be next eligible to be run by the [[event loop]]. `display(futuredata.value)`

Example promise:

```js
function display(data){
	console.log(data)
}

const futureData = fetch('https://twitter.com/will/1')
futureData.then(display);

console.log("Me first!");
```

![[fetch]] = the most powerful 5 letter word in [[javascript]]! 

#### Promises, Web APIs, the Callback and Microtask Queues and Event Loop enable:

**Non-blocking applications**: We don't have to wait in the single thread and don't block further code from running.
**However long it takes**: We cannot predict when our browser feature's work will finish so we let JS handle automatically running the funtion on its completion
**Web applications**: Asynchronous JS is the backbone of the modern web - letting us build fast 'non-blocking' applications

Promises are one way to deal with asynchronous code, without getting stuck in *callback hell*.

**Async functions** use promises behind the scenes, so understanding how promises work is fundamental to understanding how [[Async Await]] work.

## Common errors

### Uncaught TypeError: undefined is not a promise

If you get the `Uncaught TypeError: undefined is not a promise` error in the console, make sure you use `new Promise()` instead of just `Promise()`

### UnhandledPromiseRejectionWarning

This means that a promise you called rejected, but there was no `catch` used to handle the error. Add a `catch` after the offending `then` to handle this properly.

