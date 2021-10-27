---
Title: Callback functions in JS
---

## Callback functions in [[javascript]]

Callback functions are functions passed as input(arguments) to other functions. => baby 🐣 functions

```js
function doWork(function_to_do, times_to_do) {

	let result = function_to_do * times_to_do
	return result;
}

multiplyByTwo(input_num) {
	return input_num * 2;
}

doWork(multiplyByTwo(8), 3);

```

A callback is a simple function that's passed as a value to another function, and will only be executed when the event happens. We can do this because JavaScript has first-class functions, which can be assigned to variables and passed around to other functions (called **[[higher order functions]]**)

It's common to wrap all your client code in a `load` event listener on the `window` object, which runs the callback function only when the page is ready:

```js
window.addEventListener('load', () => {

 //window loaded
 //do what you want

})
```

Another example of callbacks but not using DOM event driven is when using timers:
```js
setTimeout(() => {
  // runs after 2 seconds
}, 2000)
```

## How do you handle errors with callbacks? 

One very common strategy is to use what Node.js adopted: the first parameter in any callback function is the error object: **error-first callbacks**

```js
fs.readFile('/file.json', (err, data) => {
  if (err) {
    //handle error
    console.log(err)
    return
  }

  //no errors, process data
  console.log(data)
})
```
Callbacks are great for simple cases!

However every callback adds a level of nesting, and when you have lots of callbacks, the code starts to be complicated very quickly:
```js
window.addEventListener('load', () => {
  document.getElementById('button').addEventListener('click', () => {
    setTimeout(() => {
      items.forEach(item => {
        //your code here
      })
    }, 2000)
  })
})

```

Starting with ES6, JavaScript introduced several features that help us with asynchronous code that do not involve using callbacks: [[promises]] (ES6) and [[Async Await]] (ES2017).