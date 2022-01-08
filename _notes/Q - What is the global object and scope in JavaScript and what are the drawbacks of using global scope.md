---
Title: What is the global object and scope in JavaScript? What are the drawbacks of using global scope?
---

# What is the global object and scope in JavaScript? What are the drawbacks of using global scope?

Both the browser (`window`) and Node (`global`) have a global outer scope that is managed by a global object. Variables and functions declared in the global scope are available anywhere in your code. Standard library functions we use in our program are actually stored on the global object:

```javascript
setTimeout(() => { console.log('hi') }, 1000);
// Is the same as...

// ... in the browser
window.setTimeout(() => {
  window.console.log('hi');
}, 1000);

// ... in Node
global.setTimeout(() => {
  global.console.log('hi');
}, 1000);
```

If you declare a variable or function at the top-level, it actually becomes a property on the global object.

```javascript
var a = 'hi';

function logger() {/*...*/}

a === window.a; // true
logger === window.logger; // true
```

> Declaring a variable with `let` or `const` does not add it to the global object.

We should be careful when adding variables to the global object because it can cause collisions and unexpected bugs that are hard to track down. For example, a common issue is two modules that are expecting a different value stored in the same global variable name.
# ---

Tags: #coding #interview

Topics: [[javascript]] 

