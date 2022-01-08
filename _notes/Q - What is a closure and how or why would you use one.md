---
Title: What is a closure and how/why would you use one
---

# What is a closure and how/why would you use one

Closures can be confusing for newer developers, but once the concept "clicks", they become very intuitive. The two features of JavaScript that enable closures are:

1.  Lexical scoping (variables take on values based on where they were declared)
2.  Functions as first-class citizens (specifically, a function is almost always returned as a value from another function to create the closure)

The key to forming a closure is **returning a function from another function**. This returned function now has access to all the variables of the outer function because it is part of its scope. The values of the variables persist, and can be both read and updated.

```javascript
const outerFunction = () => {
  let storedValue = 0;

  // Returning a function from another function is the key to closures
  return () => {
    storedValue = storedValue + 1;
    console.log('storedValue = ', storedValue);
  }
}

// Assign the returned function to a new variable
const newFunctionWithPermanentAccess = outerFunction();

// Execute the returned funciton to change the value it has access to (via a closure)
newFunctionWithPermanentAccess(); // storedValue = 1;
newFunctionWithPermanentAccess(); // storedValue = 2;
newFunctionWithPermanentAccess(); // storedValue = 3;
```

# ---

Tags: #coding #interview

Topics: [[javascript]] 

