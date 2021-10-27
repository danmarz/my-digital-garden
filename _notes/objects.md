---
Title: Objects in JS
---

## Objects: store functions with their associated data!

This is the principle of *encapsulation*, the following is an **object literal** AKA the full declaration of an object:
```js
const user1 = {
	name: "will",
	score: 3,
	increment: function() { user1.score++; }
};

user1.increment(); //user1.score -> 4
```

OR, alternatively, use the *'dot notation'*:
```js
const user2 = {}; //create an empty object

//assign properties to that object
user2.name = "Tim";
user2.score = 6;
user2.increment = function() {  //functions in objects are called METHODS
	user2.score++;
};

```

OR, use the *Object.create()* built-in function is [[javascript]] whose output will ALWAYS be an *empty* object:
```js
const user3 = Object.create(null);

user3.name= "Eva";
user3.score = 9;
user3.increment = function() {
	user3.score++;
};
```
* the *Object.create()* will give us some *fine-grained* controls (hidden) over our object ceation.

All these methods are repetitive and break the DRY principle, one solution would be the following:

## 1. Factory functions: Create and return objects using a function (we don't use in practice)

```js
function userCreator(name, score) {
	const newUser = {};
	newUser.name = name;
	newUser.score = score;
	newUser.increment = function() {
		newUser.score++;
	};
	return newUser;
};

const user1 = userCreator("will", 3);
const user2 = userCreator("tim", 3);
user1.increment();
```

## 2. Use the prototype chain

Store the increment function in just *one object* and have the interpreter look for it there in the one object if it doesn't find the function on user1.

We have to link user1 and functionStore so the interpreter, not finding .increment, makes sure to check the functionStore where it would find it.

*We make the link with the __Object.create()__ technique*.

```js
function userCreator(name, score) {
	const newUser = Object.create(userFunctionStore); // creates the __proto__ 
	newUser.name = name;							// link to userFunctionStore
	newUser.score = score;										|
																|
	return newUser;												|
};																|
																|
const userFunctionStore = {									  <-|
	increment: function() { this.score++; },		// JS checks for .increment()
	login: function() { console.log("Logged in"); } // this <=> user1,user2,etc.
};

const user1 = userCreator("will", 3);
const user2 = userCreator("tim", 3);
user1.increment(); // -> this: user1 IMPLICIT in its execution context
```

Turns out, EVERY object in [[javascript]] has a hidden *\_\_proto\_\_* property (by default) which is linking to THE BIG OBJECT => *Object.prototype:* { hasOwnProperty: () => {} ... etc }. 

*Object.prototype:* also has a *\_\_proto\_\_* property which is *null*. i.e. top-of-the chain.

`const userFunctionStore`'s *\_\_proto\_\_* property is overwritten by us with the *Object.create(xxxFunctionStore)* method, but we still have access to the rest of the chain UP (hasOwnProperty, etc. but indirectly)

If we change userFunctionStore's increment() function to have another function declared within, the *this.score++* within the nested *add()* function will NOT get the `this: user1` assignment of its upper class, instead it gets the not-so-useful one of the Global context, which will error as it's *score* property will be undefined.

```js
const userFunctionStore = {
	increment: function() {
		function add1(){ this.score++; }
		add1()
	}
}
```

### Arrow functions override the normal *this* rules

```js
function userCreator(name, score) {
	const newUser = Object.create(userFunctionStore); 
	newUser.name = name;							
	newUser.score = score;															
	return newUser;									
};													
													
const userFunctionStore = {
	increment: function() {
		const add1 = () => { this.score++; }
		add1()
	}
}

const user1 = userCreator("will", 3);
const user2 = userCreator("tim", 3);
user1.increment(); 
```

Inside an *arrow function* the *this* property gets its value passed by its parent's execution context!

Also DO NOT USE ARROW FUNCTIoNS on METHODS (top level functions in an object), as that will break our program(will take *this* property from global object etc.). Only use => functions in sub-functions!

## 3. The *new* keyword

All the *new* keyword does is hide under-the-hood all the no. 2 procedure (use Object.create(xxxfunctionStore), create a BOND between the object and the store via hidden proto property).. -> *new* automates all this stuff :
* creates a new object
* returns the object
* creates a name(a refrence, like newUser in our no.2 example) to refer to itself. (this!)
* creates a bond between object and functionStore (how?) 
```js
function multiplyBy2(num){
	return num*2
}

multiplyBy2.stored = 5;
multiplyBy2(3) //6

multiplyBy2.stored //5
multiplyBy2.prototype // empty {}
```
We use the fact that ALL ...
### Functions are both objects and functions
All functions have a default property *'prototype'* on their object version (itself an *object* - to replace out 'functionStore' object)

Using multiplyBy2() parenths calls the function, while
Using multiplyBy2.stored (dot) or .prototype accesses the object associated with the function.

## The new keyword automates a lot of manual work

```js
function userCreator(name, score) {
	this.name = name;
	this.score = score;
}

userCreator.prototype.increment = function(){ this.score++; };
userCreator.prototype.login = function(){ console.log("login"); };

const user1 = new userCreator("Eva", 9);

user1.increment()
```

Benefits: Faster to write. Often used in practice in professional code.
Problems: 95% of developers have no idea how it works and therefore fail interviews

We have tu upper Case first letter of the function so we know it requires 'new' to work!

## Solution 4: The `class` 'syntactic sugar'
We're writing our shared methods separately from our object 'constructor' itself (off in the `userCreator.prototype` object)

Other languages let us do this all in one place. ES2015 lets us do so too.

```js
class userCreator{
	constructor (name, score){
		this.name = name;
		this.score = score;
	}
	increment (){ this.score++; }
	login (){ console.log("login"); }
}

const user1 = new userCreator("Eva", 9);
user1.increment()
```

Benefits:
- Emerging as a new standard.
- Feels more like style of other languages ( e.g. Python )
Problems:
- 99% of developers have no idea how it works and therefore fail interviews
- But you won't 💪