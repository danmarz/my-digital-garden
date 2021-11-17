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

Starting with ES6, JavaScript introduced several features that help us with asynchronous code that do not involve using callbacks: [[promises]] (ES6) and [[Async Await]] (ES2017), so you might want to avoid using callbacks but KNOW how they work under-the-hood.

Callbacks used to be the most popular way to express and handle asynchronous functions in JavaScript programs. However, if you are still using it, I hope you already know the pain of handling multiple nested callbacks.

For example, the following code contains 4 callback functions, and it will become even harder as the code start to grow.

```js
function1(function (err, data) {   
  ...    
  function2(user, function (err, data) {  
    ...  
     function3(profile, function (err, data) {  
      ...  
      function4(account, function (err, data) {  
        ....  
      });   
    });   
  });  
});
```

As a solution, ES6 and ES7 introduced, Promises and Async/Await to handle asynchronous functions, and they are much easier to use and makes your code easily understandable to others.

But, if you use Promises or Async/Await, your code will be clean and much easy to understand.

```js
// Promises
function1()   
.then(function2)   
.then(function3)   
.then(function4)   
.catch((err) => console.error(err));

// Async/Awaitasync 

function myAsyncFunction() {    
try {      
  const data1= await function1();      
  const data2= await function2(data1);      
  const data3= await function3(data2);      
  return function4(data3);    
}   
catch (e) {      
  console.error(err);    
}}
```