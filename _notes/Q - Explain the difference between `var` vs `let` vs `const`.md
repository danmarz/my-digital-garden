---
Title: Explain the difference between `var` vs `let` vs `const`
---

# Explain the difference between `var` vs `let` vs `const`

The `var` variable declaration is typically viewed as a legacy pattern. It creates a variable that is function scoped and can be reassigned at any time. If a `var` variable is declared outside of a function, it is part of the global scope and will be a property on the global object.

```javascript
// var can be reassigned
var hello = 'world';
hello = 'Skilled.dev';
console.log(hello) // Skilled.dev

// var variable is only accessible inside the function that declared it
function test() {
  var funcScoped = 123;
  console.log(funcScoped); // 123
}
console.log(funcScoped); // ReferenceError

// var is accessible outside blocks (if statements, for loops, etc.)
if (true) {
  var worksOutsideBlocks = ':)';
  console.log(worksOutsideBlocks); // :)
}
console.log(worksOutsideBlocks) // :)
```

The `let` keyword was introduced in ES2015. A variable declared with `let` can be reassigned at any time, like a `var`.

The primary difference between `let` and `var` is that `let` is block scoped which means it is only available within a `{}` (including functions).

```javascript
// var can be reassigned
let hello = 'world';
hello = 'Skilled.dev';
console.log(hello) // Skilled.dev

function test() {
  let funcScoped = 123;
  console.log(funcScoped); // 123
}
console.log(funcScoped); // ReferenceError

if (true) {
  let blockScoped = ':)';
  console.log(blockScoped); // :)
}
console.log(blockScoped) // ReferenceError
```

The `const` keyword is used for constant variables and can only have one value ever assigned to it. An error will be thrown if we try to reassign a variable created using `const`. Like `let`, a `const` variable is block scoped.

```javascript
// const can NOT be reassigned
const hello = 'world';
hello = 'Skilled.dev'; // TypeError: Assignment to constant variable.

function test() {
  const funcScoped = 123;
  console.log(funcScoped); // 123
}
console.log(funcScoped); // ReferenceError

if (true) {
  const blockScoped = ':)';
  console.log(blockScoped); // :)
}
console.log(blockScoped) // ReferenceError
```

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

