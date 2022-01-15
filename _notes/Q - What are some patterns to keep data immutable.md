---
Title: What are some patterns to keep data immutable
---

# What are some patterns to keep data immutable

Immutability means that if we want to update a value, we create a new variable that contains the new value instead of changing a previously declared variable. Immutability is a core pattern in functional programming, and it gives us more confidence in our code because we know data cannot change throughout the lifetime of the program.

JavaScript doesn't offer immutable arrays or objects by default, but there are functions we can use to prevent objects from being altered and patterns we can follow to prevent data from being mutated. Strings in JavaScript are immutable.

Some examples of maintaining immutability in JS are:

```javascript
// Spread operator in arrays
const arr1 = [1, 2, 3];
const arr2 = [...arr1];
// This creates a brand new array with the same values
arr1 === arr2; // false

// Spread operator in objects
const obj1 = { hi: 'world' };
const obj2 = { ...obj1 };
// This creates a brand new object with the same properties
obj1 === obj2;// false

// Object.assign
const obj1 = { hi: 'world' };
const obj2 = Object.assign({}, obj1);
// This creates a brand new object with the same properties
obj1 === obj2; // false

// Array.slice
const arr1 = [1, 2, 3];
const arr2 = arr1.slice(1);
console.log(arr1); // [1, 2, 3]
console.log(arr2) ;// [2, 3]
```

> Note that Object.assign and the spread operator just make a shallow copy of the object. If you have nested arrays and/or objects, they will still point to the original value.

JavaScript also offers a lower-level configuration of objects.

You can use `Object.defineProperty` to set the CRUD access on individual properties. Setting `writable: false` means that you can't change the value of a property, and setting `configurable: false` means you can't change the type or delete it from the object.

```javascript
const obj = {};

Object.defineProperty(obj, 'hello', {
  value: 'world',
  writable: false,
  configurable: false,
});

console.log(obj.hello); // 'world'
obj.hello = 'Skilled.dev';
console.log(obj.hello); // 'world'
```

You can use `Object.preventExtensions` to block an object from adding new properties.

```javascript
var obj = {
  hello: 'world',
};

Object.preventExtensions(obj);

obj.test = 123;
console.log(obj.test); // undefined

// You are still allowed to change existing properties
obj.hello = 'Skilled.dev';
console.log(obj) // { hello: 'world' }
```

You can use `Object.seal` which is the same as `Object.preventExtensions`, but it also sets all the properties to `configurable: false`. You cannot add new properties, change the type of existing properties, or delete properties.

```javascript
var obj = {
  hello: 'world',
};

Object.seal(obj);

delete obj.hello;

console.log(obj); // { hello: 'world' }
```

Using `Object.freeze` gives you the highest level of immutability. You cannot add, change, or delete properties.

```javascript
var obj = {
  hello: 'world',
};

Object.freeze(obj);

obj.test = 123;
obj.hello = 'Skilled.dev';
delete obj.hello;

console.log(obj); // { hello: 'world' }
```
# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

