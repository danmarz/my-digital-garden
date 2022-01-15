---
Title: Explain the `bind`, `call`, and `apply` functions
---

# Explain the `bind`, `call`, and `apply` functions

All three of these methods can be used to set the `this` value of a function. There are some subtle differences in how they work though.

### `bind`

This is used to set the `this` of a function, but does not invoke the function immediately. Instead, it returns a **new function** with the `this` value set. So the value returned is a function you can execute later where the `this` is forced to take the value of the object that you called with `bind`.

```javascript
function sayHello() {
  console.log(`Hello, ${this.name}!`);
}

const bob = { name: 'Bob' };

// It creates a new function with its `this` set
const sayHelloToBob = sayHello.bind(bob);
// We can then execute the function later
sayHelloToBob(); // Hello, Bob!
```

### `call`

The `call` method will execute a function immediately, and the first parameter is the object that is set as `this`. All the arguments after it will be passed to the function.

```javascript
function greetFriend(firstName, lastName) {
  console.log(`Hello, ${this.name}! Nice to meet you ${firstName} ${lastName}`);
}

const bob = { name: 'Bob' };

// It executes the function with `this` and passes params
greetFriend.call(bob, 'Dan', 'McCool');
```

### `apply`

The `apply` method will execute a function immediately, and the first parameter is the object that is set as `this`. The second parameter is an array whose items will each be passed to the function as arguments.

```javascript
function greetFriend(firstName, lastName) {
  console.log(`Hello, ${this.name}! Nice to meet you ${firstName} ${lastName}`);
}

const bob = { name: 'Bob' };

// It executes the function with `this` and passes params
greetFriend.apply(bob, ['Dan', 'McCool']);
```

### `call` vs. `apply`

Both of these execute the function immediately.

The primary difference is that `call` will take an arbitrary number of arguments. The first parameter is the value set to `this`. The remaining arguments will be used as parameters in the executed function.

The `apply` method only takes **two arguments**. The first argument is the value to set `this`. The second argument is an array that is passed to the executed function as individual parameters.

Sometimes these methods are simply used to execute a function, and if the `this` content is not necessary, you can use `null` as the first argument.
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

