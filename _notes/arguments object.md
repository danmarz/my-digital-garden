---
Title: The arguments object
---

The arguments object is an `Array`-like object accessible inside functions that contains the values of the arguments passed to that function.

1. **How can I use this?**
	If a function has 3 arguments for example:
	```js
	arguments[0] // first argument
	arguments[1] // second argument
	arguments[2] // third argument
	```
	The `arguments` object is not an `array`. It is similar, but lacks all `Array` properties except `.length`. For example, it doesn't have the `pop()` method.
	
	However, it can be converted to a real `Array`:
	
	```js
	let args = Array.prototype.slice.call(arguments);
	// Using an array literal is shorter than above but allocates an empty array
	let args = [].slice.call(arguments)
	```

	You can also use ES2015's `Array.from()` method or [[spread syntax]] to convert `arguments` to a real Array:
	```js
	let args = Array.from(arguments);
	// or
	let args = [...arguments];
	```
1. **Why must I use this?**
	- You can use `arguments.length` to count how many arguments the function was called with

3. **When will I use this?**
	Each argument can be set or reassigned:
	```js
	arguments[1] = 'new value'
	```

# ---

Tags: #
Topics: [[javascript]]

