---
Title: What is the difference between these function declarations
---

# What is the difference between these: `function Car(){}`, `const car = Car()`, and `const car = new Car()`

-   `function Car(){}` declares a function
-   `const car = Car()` executes a function and assigns its return value (which can be anything) to the `car` variable
-   `const car = new Car()` creates an instance of a `Car` object and assigns this object to `car`

It's also worth noting that we could do something like `const car = () => {}` or `const car = function(){}` which assigns a function expression to a variable.
# ---

Tags: #coding #interview

Topics: [[javascript]] 

