---
title: Javascript
---

JavaScript is a loosely typed language, meaning you don’t have to specify what type of information will be stored in a variable in advance. JavaScript automatically types a variable based on what kind of information you assign to it (e.g., that '' or " " to indicate string values). Many other languages, like [[Java]], require you to declare a variable’s type, such as int, float, boolean, or String.

## There are 9 primitive types in Javascript

* Undefined -> Things we (accidentaly) left undefind
* Null -> Things we intentionally left null
* Numbers -> numbers, floats, negative, positive etc.
* String -> any " "
* Boolean -> true and false
* BigInt -> Rather new and unused much, *science of big numbers*
* Symbol -> New and uncommon

### And then Objects and Functions
* Objects -> used to group related data and code
* Functions -> used to refer to code

## NO OTHER TYPES BESIDE THESE 9

## ~~Everything is an object in JavaScript~~
> In JavaScript, _everything_ is an object, even when it’s something else. Functions are objects. Strings are objects. Numbers are objects. Arrays are objects. Objects are objects. **You can assign properties to basically anything in JS** 

The above is not correct. Weird, right? Everything SEEMS to be an object, but it's just because of the inner workings of JS, i.e. creaing a wrapper `String` object from `s`, equivalent to using `new String(s)`. This also happens to Numbers and Booleans.

Functions, however are fully fledged objects and inherit from `Object`, actually from `Object.prototype`. Functions therefore can do anything objects can, *including having properties*.

## Expressions are questions that JS can answer  ― like ( 2 + 2), in the only way it knows how  ― with *values*. 
 Expressions always result in a single value, i.e. (2 + 2) -> 4
 
 ## On classes
 JavaScript is a **prototype-based language**, meaning object properties and methods can be shared through generalized objects that have the ability to be cloned and extended. This is known as prototypical inheritance and differs from class inheritance. Among popular object-oriented programming languages, JavaScript is relatively unique, as other prominent languages such as **PHP, Python, and Java are class-based languages**, which instead define classes as blueprints for objects.
 
 ![[closure]]
 
 ## Callback functions
 
 - Are functions that have functions passed as parameters i.e. map(\[array\], function), filter, reduce..etc. A core aspect of functional programming
 - Makes our code more declarative and readable:
 `const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);`
 - **Asynchronous Javascript**: Callbacks are a core aspect of async JavaScript, and are under-the-hood of *promises*, *async/await*
 
 ## Arrow functions - a shorthand way to save functions
 
 `function multiplyBy2(input) { return input * 2; }`
 `const multiplyBy2 = (input) => { return input*2 }`
 `const multiplyBy2 = (input) => input*2`
 `const multiplyBy2 = input => input*2` //No () if `input` is single param
 `const output = multiplyBy2(3)	// 6`
 
 #### Updating callback function with an arrow function ➡
 
 `const multiplyBy2 = input => input*2`
 `const result = copyArrayAndManipulate([1,2,3], multiplyBy2);`
 
 You can even pass the arrow function directly and save assigning a label to it.
 
 `const result = copyArrayAndManipulate([1,2,3], input => input*2)`
 
 As seen above, *anonymous* and *arrow* functions improve immediate legibility of the code, but can cause confusion if they are not well understood.