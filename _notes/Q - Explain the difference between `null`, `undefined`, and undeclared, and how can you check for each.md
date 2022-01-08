---
Title: Explain the difference between `null`, `undefined`, and undeclared, and how can you check for each
---

# Explain the difference between `null`, `undefined`, and undeclared, and how can you check for each

These three values may seem similar but actually have very different impacts on our code.

-   `null`: declared and **explicitly assigned** an empty value
-   `undefined`: declared and **not assigned** a value
-   undeclared: trying to use a variable that was never declared

### **`null`**

If a variable is set to `null`, we are **explicitly saying this value is empty**.

Properties in JSON objects can be set to `null`, and it will persist through an HTTP request. If another programming language uses the same JSON object, they will have their own `null` construct to handle the value.

In JavaScript, if we have default function parameters and pass a `null` input, it will use the `null` and not the default.

```javascript
const explicitlyEmpty = null;

// Serializes to JSON
JSON.stringify({ explicitlyEmpty }); // "{ "explicitly": null }"

// Does not trigger default parameters
const sayHello = (name = 'world!') => {
  console.log(`Hello, ${name}`);
}
sayHello(explicitlyEmpty);

// Checking for null
explicitlyEmpty === null; // true
```

### **`undefined`**

An `undefined` variable means it has been declared but not assigned a value (as opposed to `null` which has explicitly assigned the value).

If there is an `undefined` value in an object that is converted to JSON, it will remove this property.

When using default function parameters, if there is no value passed, it is viewed as `undefined` and will use the default value.

> Developers often use `typeof x === 'undefined'` to check for `undefined` because it won't throw an error if the variable is actually undeclared.

```javascript
let empty;

// Does NOT serialize to JSON
JSON.stringify({ empty }); // "{}"

// Does not trigger default parameters
const sayHello = (name = 'world!') => {
  console.log(`Hello, ${name}`);
}
sayHello(empty);

// Checking for null
empty === undefined; // true
typeof empty === 'undefined'; // true
```

### undeclared

Undeclared is the terminology we use to describe when we try to use a variable in our code that was never declared at all.

```javascript
// We will get a ReferenceError because x is not declared
x + 1; // ReferenceError

// It still throws a reference error if we try to do this
x === undefined; // ReferenceError

// This works with undeclared variables
typeof x === 'undefined'; // true
```

# ---

Tags: #coding #interview

Topics: [[javascript]] 

