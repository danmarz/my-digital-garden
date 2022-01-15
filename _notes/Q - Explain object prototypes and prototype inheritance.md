---
Title: Explain object prototypes and prototype inheritance
---

# Explain object prototypes and prototype inheritance

This is a deep topic that takes a book to understand fully. I'll give a synopsis of how to understand it for an interview, but I highly recommend ["YDKJS: `this` & Object Prototypes"](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/this%20&%20object%20prototypes/README.md#you-dont-know-js-this--object-prototypes) for completeness. Plus it's free on GitHub!

To understand prototypes, it's probably best to first explain class inheritance vs. prototype inheritance.

### Classes (in Classical Object-Oriented Languages)

A class is how most other object-oriented languages (such as Python, Java, C++) share functionality. Classes are a blueprint for how to create an object, and to inherit its functionality in a child class, you **copy all of the methods and properties over**. They are then duplicated in the child.

Classes themselves are not objects, but define the properties and methods an object will have.

> NOTE: The JavaScript `class` keyword is not an actual class like in other languages. It is still just a wrapper around prototypes but uses syntax that will feel similar to other languages. Declaring a `class` is actually just a constructor function that makes it easier to manage properties and the prototype.

### Prototypes

In JavaScript, we don't use classes as a blueprint. **We use objects themselves to share properties and methods**. This means we do not make copies. Instead we link to an object through the prototype chain to indicate we want to utilize a parent's functionality.

Every object in JavaScript has a `__proto__` property which points to the object it inherits from. The object it points to also has its own `__proto__`, which points to its parent. This is how we create the prototype chain — each object points to the object it inherits from until we reach the `Object` that sits at the top of the prototype chain.

If we try to call a method or property on an object we create, but it doesn't exist on our object directly, JavaScript will then walk through the objects in the prototype chain to see if any of them have it.

### Benefits of Prototypes

One of the primary benefits of using prototypes over classes is that we don't duplicate properties on children, and this drastically reduces the memory footprint. We just have a single object for each level of inheritance that is referred to when we need functionality. For example, when you create a React class component `class MyComponent extends React.Component`, each one just points to the `Component` object instead of copying over all the methods for _every single_ component you make in your app.

Another benefit is that there is less coupling. We can compose objects to group functionality instead of bolting it to class. This approach can actually be much simpler and also more powerful, once you understand prototypes and how they work.

Prototypes are also easily extended. For example, when a new version of JavaScript is released, we can easily modify and polyfill new functions on existing objects.

### Prototype Example

An easy way to use prototype inheritance and set an object's `__proto__` is through the `Object.create` function. It creates a new object with the `__proto__` set to the value passed to `create`.

```javascript
const person = {
  getName() {
    return this.name;
  },
  getAge() {
    return this.age;
  }
}

// This sets bob.__proto__ = person;
const bob = Object.create(person);
// This sets dan.__proto__ = person;
const dan = Object.create(person);

console.log(bob.__proto__); // { getName, getAge }
console.log(dan.__proto__); // { getName, getAge }

// They have the same __proto__ (not a copy)
console.log(bob.__proto__ === person); // true
console.log(dan.__proto__ === bob.__proto__); // true

bob.name = 'Bob';
bob.age = 42;
console.log(`My name is ${bob.getName()} and I am ${bob.getAge()}`);

// If we change person, the objects that inherit from it will receive the properties/methods
person.TEST = '123';

console.log(bob.TEST);
```

### `new` and `prototype`

A final note is that `prototype` is actually a property of **functions** that are used as constructors. For all objects created using the `new` keyword with this constructor function, their `__proto__` will point to the `prototype` of the constructor function.

```javascript
function Person(name, age) {
  // this is automatically return when Person is used as a constructor
  this.name = name;
  this.age = age;
}

// `prototype` is a property of functions
// and will be passed to objects created using `new`
Person.prototype.getName = function() { return this.name };
Person.prototype.getAge = function() { return this.age };

console.log(Person.prototype); // { constructor, getName, getAge }

const bob = new Person('Bob', 42);
console.log(`My name is ${bob.getName()} and I am ${bob.getAge()}`);

console.log(bob.__proto__ === Person.prototype); // true

Person.prototype.TEST = 123;

console.log(bob.TEST); // 123
```

This would be very similar to using a `class` in JavaScript since a `class` is just a constructor function that helps us manage objects and the prototype chain. In fact, you could run the same code above and just replace the `function Person(name, age) { /* ... */ }` constructor with the `class Person { /* ... */ }` class.

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

If we wanted to use inheritance, we could use the `extends` syntax, like `class Friend extends Person {/* ... */}`. This helps us manage the prototype chain underneath the hood.

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

