---
Title: JS snippets
---


# [[javascript]] Timers
## setTimeout()

Delays the execution of a function. You specify a callback function to execute later and the time to wait before exec in miliseconds.
```js
setTimeout(() => {
  // runs after 2 seconds
}, 2000)

setTimeout(() => {
  // runs after 50 milliseconds
}, 50)
```

Instead of anonymous functions you can pass an existing function name + param(s):
```js
const myFunction = (firstParam, secondParam) => {
  // do something
}

// runs after 2 seconds
setTimeout(myFunction, 2000, firstParam, secondParam)
```
`setTimeout()` returns the timer ID. You can store this ID and clear it if you want to delete this scheduled function execution:
```js
const id = setTimeout(() => {
  // should run after 2 seconds
}, 2000)

// I changed my mind
clearTimeout(id)
```
If you specify the timeout delay to `0`, the callback function will be executed as soon as possible, but after the current function execution. (**equivalent** to `setImmediate()`, both will be executed in the next iteration of the event loop.)

**NOTE:** A function passed to `process.nextTick()` is going to be executed on the current iteration of the event loop, after the current operation ends. This means it will always execute before `setTimeout` and `setImmediate`.

## setInterval()

Is similar to `setTimeout` but with a difference: instead of running the callback function once, it will run it forever, at the specific time interval you specify in miliseconds.
```js
const id = setInterval(() => {
  // runs every 2 seconds
}, 2000)

clearInterval(id)
```

Another common way to use `setInterval` is to check for *something* before the loop:
```js
const interval = setInterval(() => {
  if (App.somethingIWait === 'arrived') {
    clearInterval(interval)
    return
  }
  // otherwise do things
}, 100)
```

If a function always takes the same amount of time to complete, all is fine! But if your function has different execution times, something like *function A1* --> *function A2* -> -> -> *function A3* then you can chedule a recursive `setTimeout` to be called when the callback function finishes:

```js
const myFunction = () => {
	// do something
	
	setTimeout(myFunction, 1000)
}
setTimeout(myFunction, 1000)
```

# JS promises

A promise is commonly defined as **a proxy for a value that will eventually become available**.

```js
let done = true;

const isItDoneYet = new Promise((resolve, reject) => {
 if (done) {
 	const workDone = 'Here is the thing I built';
	resolve(workDone);

 } else {
	 const why = 'Still working on something else';
	 reject(why);
 }
});

const checkIfItsDone = () => {
 isItDoneYet
 .then((ok) => {
	 console.log(ok);
 })
 .catch((err) => {
	 console.error(err);
 });
};

checkIfItsDone();
```
Running `checkIfItsDone()` will specify functions to execute when the `isItDoneYet` promise resolves (in the `then` call) or rejects (in the `catch` call).

**Promisifying**: This technique is a way to be able to use a classic JavaScript function that takes a callback, and have it return a promise:
```js
const fs = require('fs')

const getFile = (fileName) => {
  return new Promise((resolve, reject) => {
    fs.readFile(fileName, (err, data) => {
      if (err) {
        reject(err)  // calling `reject` will cause the promise to fail with or without the error passed as an argument
        return        // and we don't want to go any further
      }
      resolve(data)
    })
  })
}

getFile('/etc/passwd')
.then(data => console.log(data))
.catch(err => console.error(err))
```

### Example of chaining promises
```js
const status = response => {
  if (response.status >= 200 && response.status < 300) {
    return Promise.resolve(response)
  }
  return Promise.reject(new Error(response.statusText))
}

const json = response => response.json()

fetch('/todos.json')
  .then(status)    // note that the `status` function is actually **called** here, and that it **returns a promise***
  .then(json)      // likewise, the only difference here is that the `json` function here returns a promise that resolves with `data`
  .then(data => {  // ... which is why `data` shows up here as the first parameter to the anonymous function
    console.log('Request succeeded with JSON response', data)
  })
  .catch(error => {
    console.log('Request failed', error)
  })
```
In this example, we call `fetch()` to get a list of TODO items from the `todos.json` file found in the domain root, and we create a chain of promises.

Running `fetch()` returns a response, which has many properties, and within those we reference:

-   `status`, a numeric value representing the HTTP status code
-   `statusText`, a status message, which is `OK` if the request succeeded

`response` also has a `json()` method, which returns a promise that will resolve with the content of the body processed and transformed into JSON.

So given those promises, this is what happens: the first promise in the chain is a function that we defined, called `status()`, that checks the response status and if it's not a success response (between 200 and 299), it rejects the promise.

This operation will cause the promise chain to skip all the chained promises listed and will skip directly to the `catch()` statement at the bottom, logging the `Request failed` text along with the error message.

If that succeeds instead, it calls the `json()` function we defined. Since the previous promise, when successful, returned the `response` object, we get it as an input to the second promise.

In this case, we return the data JSON processed, so the third promise receives the JSON directly:
```js
.then((data) => {
  console.log('Request succeeded with JSON response', data)
})
```
and we simply log it to the console.

### Orchestrating promises

## Promise.all()

If you need to synchronize different promises, `Promise.all()` helps you define a list of promises, and execute something when they are all resolved.

```js
const f1 = fetch('/something.json')
const f2 = fetch('/something2.json')

Promise.all([f1, f2])
  .then(res => {
    console.log('Array of results', res)
  })
  .catch(err => {
    console.error(err)
  })
```

The ES2015 destructuring assignment syntax allows you to also do
```js
Promise.all([f1, f2]).then(([res1, res2]) => {
  console.log('Results', res1, res2)
})
```
You are not limited to using `fetch` of course, **any promise can be used in this fashion**.

## Promise.race()

`Promise.race()` runs when the first of the promises you pass to it settles (resolves or rejects), and it runs the attached callback just once, with the result of the first promise settled.

```js
const first = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'first')
})
const second = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'second')
})

Promise.race([first, second]).then(result => {
  console.log(result) // second
})
```

# Async/Await

This is a simple example of *async/await* used to run a function asynchronously:
```js
const doSomethingAsync = () => {
 return new Promise(resolve => {
 setTimeout(() => resolve('I did something'), 3000)
 })
}

const doSomething = async () => {
 console.log(await doSomethingAsync())
}

console.log('Before')
doSomething()
console.log('After')
```

# EventEmitter()
```js
import EventEmitter from 'events'
const eventEmitter = new EventEmitter(); //emmiter object
```

This object exposes, among many others, the `on` and `emit` methods.
-   `on` is used to add a callback function that's going to be executed when the event is triggered
```js
eventEmitter.on('start', () => {
  console.log('started')
})
```
-   `emit` is used to trigger an event
```js
eventEmitter.emit('start')
```

# HTTP Server
```js
import http from 'http';
  
const port = process.env.PORT || 3000
  
const server = http.createServer((req, res) => {

 res.statusCode = 200
 res.setHeader('Content-Type', 'text/html')
 res.end('<h1>Hello, World!</h1>')
})
  
server.listen(port, () => {
 console.log(`Server running at port ${port}`)
})
```

The callback function we pass is the one that's going to be executed upon every request that comes in. Whenever a new request is received, the `request` event is called, providing two objects: a request (an `http.IncomingMessage` object) and a response (an `http.ServerResponse` object).
- `request` provides the request details. Through it, we access the request headers and request data.
- `response` is used to populate the data we're going to return to the client.

## Perform a GET Request
```js
const https = require('https')
const options = {
  hostname: 'example.com',
  port: 443,
  path: '/todos',
  method: 'GET'
}

const req = https.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`)

  res.on('data', d => {
    process.stdout.write(d)
  })
})

req.on('error', error => {
  console.error(error)
})

req.end()
```

## Perform a POST Request
```js
const https = require('https')

const data = new TextEncoder().encode(
  JSON.stringify({
    todo: 'Buy the milk 🍼'
  })
)

const options = {
  hostname: 'whatever.com',
  port: 443,
  path: '/todos',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length
  }
}

const req = https.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`)

  res.on('data', d => {
    process.stdout.write(d)
  })
})

req.on('error', error => {
  console.error(error)
})

req.write(data)
req.end()
```

PUT and DELETE requests use the same POST request format - you just need to change the `options.method` value to the appropriate method.

There are many ways to perform an HTTP POST request in Node.js, depending on the abstraction level you want to use.

The simplest way to perform an HTTP request using Node.js is to use the [Axios library](https://github.com/axios/axios):

```js
import axios from 'axios';

axios                           // POST
  .post('https://whatever.com/todos', {
    todo: 'Buy the milk'
  })
  .then(res => {
    console.log(`statusCode: ${res.status}`)
    console.log(res)
  })
  .catch(error => {
    console.error(error)
  })
  
axios                          // GET
  .get('https://google.com/')
  .then(function (response) {  // handle success
    console.log(response);
  })  
```

## Get HTTP request body data using Node.js

If you are using Express, that's quite simple: use the `body-parser` Node.js module.

For example, to get the body of this request:
```js
import axios from 'axios';

axios.post('https://whatever.com/todos', {
  todo: 'Buy the milk'
})
```

This is the matching server-side code:
```js
const express = require('express')
const app = express()

app.use(
  express.urlencoded({
    extended: true
  })
)

app.use(express.json())

app.post('/todos', (req, res) => {
  console.log(req.body.todo)
})
```

If you're not using Express and you want to do this in vanilla Node.js, you need to do a bit more work, of course, as Express abstracts a lot of this for you.

The key thing to understand is that when you initialize the HTTP server using `http.createServer()`, the callback is called when the server got all the HTTP headers, but not the request body.

The `request` object passed in the connection callback is a stream.

So, we must listen for the body content to be processed, and it's processed in chunks.

We first get the data by listening to the stream `data` events, and when the data ends, the stream `end` event is called, once:

```js
const server = http.createServer((req, res) => {
  // we can access HTTP headers
  req.on('data', chunk => {
    console.log(`Data chunk available: ${chunk}`)
  })
  req.on('end', () => {
    //end of data
  })
})
```

So to access the data, assuming we expect to receive a string, we must concatenate the chunks into a string when listening to the stream `data`, and when the stream `end`, we parse the string to JSON:

```js
const server = http.createServer((req, res) => {
  let data = '';
  req.on('data', chunk => {
    data += chunk;
  })
  req.on('end', () => {
    console.log(JSON.parse(data).todo); // 'Buy the milk'
    res.end();
  })
})
```

Starting from Node.js v10 a `for await .. of` syntax is available for use. It simplifies the example above and makes it look more linear:

```js
const server = http.createServer(async (req, res) => {
  const buffers = [];

  for await (const chunk of req) {
    buffers.push(chunk);
  }

  const data = Buffer.concat(buffers).toString();

  console.log(JSON.parse(data).todo); // 'Buy the milk'
  res.end();
})
```

# Exiting a Node.js program

The `process` core module provides a handy method that allows you to programmatically exit from a Node.js program: `process.exit()`.

When Node.js runs this line, the process is immediately forced to terminate.

This means that any callback that's pending, any network request still being sent, any filesystem access, or processes writing to `stdout` or `stderr` - all is going to be ungracefully terminated right away.

If this is fine for you, you can pass an integer that signals the operating system the exit code:

```js
process.exit(1)
```

By default the exit code is `0`, which means success. Different exit codes have different meaning, which you might want to use in your own system to have the program communicate to other programs.

You can also set the `process.exitCode` property:

```js
process.exitCode = 1
```

and when the program ends, Node.js will return that exit code.

`SIGKILL` is the signal that tells a process to immediately terminate, -> `process.exit()`.

### The recommended exit method is 'SIGTERM'

`SIGTERM` is the signal that tells a process to gracefully terminate. It is the signal that's sent from process managers like `upstart` or `supervisord` and many others.

You can send this signal from inside the program, in another function:
```js
process.kill(process.pid, 'SIGTERM')
```