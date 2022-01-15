---
Title: Explain hoisting in JavaScript
---

# Explain hoisting in JavaScript

Variables declared using the `var` keyword or functions declared using the `function` keyword will have their declarations moved to the top of their scope.

```javascript
// Hoisting
console.log(hello) // undefined
var hello = 'world';
console.log(hello) // world

// This is equivalent to
var hello;
console.log(hello) // undefined
hello = 'world';
console.log(hello) // world
```

So in the example above, even though we declared `hello` after the first `console.log`, it still recognizes the variable as existing but just `undefined`. Hoisting means JavaScript actually moves the `var hello;` to the top of the file which gives it an initial value of `undefined`. Then when the code execution actually reaches the original `var hello = 'world';` it just assigns the value for the first time.

This is in contrast to `let` and `const`, which are not hoisted. You will receive a reference error saying the variables are not defined and the program will crash.

```javascript
// const and let are NOT hoisted, so we get an error
console.log(greet, friend) // ReferenceError: Cannot access 'greet' and 'friend' before initialization
const greet = 'hello';
let friend = 'world';

console.log(greet, friend) // This never runs because the code crashed with an error
```

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

