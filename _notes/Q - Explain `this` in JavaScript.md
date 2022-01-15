---
Title: Explain `this` in JavaScript
---

# Explain `this` in JavaScript

The `this` keyword in JavaScript is one of the language's' most difficult topics. Even if a developer knows the theory behind `this`, it can still be challenging to work with in real code. Explaining `this` fully would [require a book](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed/this%20%26%20object%20prototypes).

The `this` keyword allows us to refer to the object that executes a method.

```javascript
class User {
  constructor() {
    this.username = '';
  }

  changeUsername(username) {
    this.username = username;
  }

  getUsername() {
    return this.username;
  }

  sayHello() {
    console.log(`Hello, ${this.getUsername()}`);
  }
}

const skilledUser = new User();
skilledUser.changeUsername('Skilled.dev');
skilledUser.sayHello();
```

In the example above, we create an object and use the `this` keyword inside of it to reference the object itself. It allows us to access data and methods on the object.

The most difficult part of understanding `this` in JavaScript is that its value is determined by _where the function is called that uses `this`_. So even if you define `this` to work a certain way, it can still change at any point in your program. There are six rules to help you determine what the value of `this` will be.

1.  When you create an object using the `new` keyword with a constructor function/`class`, `this` will refer to the new object inside the function.
2.  Using `bind`, `call`, or `apply` will override the value inside a function, and you can hardcode its value for `this`.
3.  If a function is called on an object as a method, `this` will refer to the object that is calling it. For example, `myObject.method()` would have a value of `this` that refers to `myObject`.
4.  If a function is executed without any of the three previous criteria being applied, `this` will refer to the global object, which is `window` in the browser or `global` in Node. If you are using strict mode, `this` will be `undefined` instead of the global object.
5.  If multiple rules from above apply, it will use the rule that comes first in this list.
6.  Arrow functions ignore all the above rules, and the value of `this` is determined by the scope enclosed by the arrow function.

In interviews, what you will most often be tested on is:

-   Giving a simple explanation of `this`
-   Set the value of `this` using one of the JavaScript rules
-   Fix code where `this` isn't working as expected
-   Build an object that uses `this` inside it

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

