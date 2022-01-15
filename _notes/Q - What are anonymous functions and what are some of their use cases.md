---
Title: What are anonymous functions and what are some of their use cases
---

# What are anonymous functions and what are some of their use cases

In JavaScript, functions are first-class citizens, which means they can be treated like any other data type. A function can be assigned to a variable, passed as an argument to another function, or be the return value from another function.

An anonymous function means we use a function without declaring or naming it. The most common example of this in JavaScript is a callback function where we pass an anonymous function as an argument to be executed inside another function.

```javascript
// Array.prototype.map: arr.map(callback)
const arr = [1, 2, 3];
const squaredArray = arr.map((item) => item * item); // [1, 4, 9]
// (item) => item * item is an anonymous function callback
// It is executed on each item

// setTimeout(callback, milliseconds)
setTimeout(function() {
  alert('I am alerting in a callback')
}, 1000);
// After 1000ms, the setTimeout function executes the anonymous function callback
```

If a variable is declared in the global scope, it is available to all the code in an app, which can cause collisions and difficult bugs. JavaScript is a function-scoped language which means that variables declared in a function will only exist in that function. Anonymous functions declared as [IIFEs](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) would allow us to run code without polluting the global scope.

```javascript
(function() {
  // a is only accessible in this scope
  var a = 1;
})();
```
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

