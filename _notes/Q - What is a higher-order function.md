---
Title: What is a higher-order function
---

# What is a higher-order function

JavaScript treats functions as first-class citizens which allows for some very powerful patterns. A higher-order function takes another function as an argument and/or its return value is also function.

Common examples of this include:

-   Anonymous function arguments
-   Closures
-   A callback function to handle the result of an asynchronous call
-   A callback to dictate how a function should execute, such as `arr.map(x => x * x)`
-   Function currying, where the returned value is another function that can subsequently be executed

```javascript
// Curry the add function
const add = a => b => a + b;
const add5 = add(5);

add5(10) // 15
add5(37) // 42
add5(0) // 5

// Execute both sequentially
add(1)(2) // 3
```
# ---

Tags: #coding #interview

Topics: [[javascript]] 

