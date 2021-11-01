---
title: Understanding `this` in JavaScript
---

# Understanding `this` in JavaScript - DEV Community

If you're coming to JavaScript from an Object-Oriented language like Ruby or Python, the concept of `this` in JavaScript might give you a little bit of trouble. It turns out, `this` in JavaScript is a little more complex than we might think assuming it to work similar to `self`.

Let's take a step back and think about `self` in an OO language. We know `self` will always to the current instance of a class, and that when defining a class method, `this` refers to the class itself.

## What's different in JavaScript?

Let's first back up and define what type of language JavaScript is. Yes, we can create "objects" in JavaScript, but it is only through [prototypal inheritance](https://www.educative.io/blog/understanding-and-using-prototypal-inheritance-in-javascript#:~:text=Simply%20put%2C%20prototypical%20inheritance%20refers,inherit%20properties%20from%20a%20prototype).

In an **object method** in JavaScript, `this` refers to the object invoking the function, not the instance of an object.

Let's try the following code in the console:  

```js
function whatIsThis() {
  return this;
}

whatIsThis()
// => Window {window: Window, self: Window, document: document, name: '0.980485403589378', location: Location, …}
```

The return value is the global object, in the console, this is the `Window` object. Simply put, what object is invoking our function `whatIsThis`, it is the global object, where all of our other code lives.

Let's try dig a little deeper and start building an object with some functionality. Here, we create a `person` object and give it a function to return the full name.  

```js
const person = {
  firstName: "Karson",
  lastName: "Kalt",
  id: 2948,
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
};

person.fullName()
// => 'Karson Kalt'
```

The method `.fullName()`, is called by the `person` object, so in this case, `this` refers to the object `person`. So far so good right? Well, let's keep exploring, this time invoking a callback function.

In the following example, we have a `person` object who has an attribute of `pets`. What if we want to create a method that prints to the console the name of the person and the name of the pet?  

```js
const person = {
  firstName: "Karson",
  lastName: "Kalt",
  pets: ["Jazz the Dog", "Maniac the Cat", "Seymour the Turtle"],
  id: 2948,
  allPets: function() {
    this.pets.forEach(function(pet) {
      console.log(this.firstName + " has a pet named "+ pet)
    })
  }
};

person.allPets()
// The console prints the following:
// undefined has a pet named Jazz the Dog
// undefined has a pet named Maniac the Cat
// undefined has a pet named Seymour the Turtle
```

So why are we able to iterate through each `pet`, but the `firstName` attribute becomes undefined. Let's think about a core principle of JavaScript -- hoisting. This is a core part of JavaScript, any of our function declarations are first stored in memory before any code is run. This allows us to use a function before the actual line it appears on, but the function now lives globally. Therefore, the global object is the one executing `function(pet)`, and does not have an attribute defined for `firstName`.

So, in reality `function(pet)` is not a method of our `person` object, the only method assigned to `person` is `allPets`.\\

A quick and easy way to fix this issue, is to tell the `.forEach` method what `this` is referring to, by passing it an optional second argument that defines what `this` is inside the iterator. Within the scope of `this.pets`, `this` refers to the object itself, so we can pass `.forEach` an argument of `this`.  

```js
const person = {
  firstName: "Karson",
  lastName: "Kalt",
  pets: ["Jazz the Dog", "Maniac the Cat", "Seymour the Turtle"],
  id: 2948,
  allPets: function() {
    this.pets.forEach(function(pet) {
      console.log(this.firstName + " has a pet named "+ pet)
    }, this)
  }
};

person.allPets()
// The console prints the following:
// Karson has a pet named Jazz the Dog
// Karson has a pet named Maniac the Cat
// Karson has a pet named Seymour the Turtle
```

Since ES6, we quickly create functions that bind `this` where they are declared, by using an arrow function instead:  

```js
const person = {
  firstName: "Karson",
  lastName: "Kalt",
  pets: ["Jazz the Dog", "Maniac the Cat", "Seymour the Turtle"],
  id: 2948,
  allPets: function() {
    this.pets.forEach(pet => {
      console.log(this.firstName + " has a pet named "+ pet)
    })
  }
};

person.allPets()
// The console prints the following:
// Karson has a pet named Jazz the Dog
// Karson has a pet named Maniac the Cat
// Karson has a pet named Seymour the Turtle
```

Source: https://dev.to/karsonkalt/understanding-this-in-javascript-oe2
Tags: 
Topics: [[javascript]]


