---
Title: Named vs Arrow functions
---

1. **How can I use this?**
Named function syntax (AKA function declaration):
	-	`function sum(x, y){ return x + y; }`
	Anonymous function syntax:
	-	`const sum = function(x, y){ return x + y; }`
	Arrow function syntax (AKA [[Lambda functions]] in Python):
	-	`const sum = (x, y) =>  return x + y`

2. **Why must I use this?**
	1. Named functions are *hoisted* which means they are loaded into memory at compilation and allows calls to functions to be present in code before the function declaration itself.
	2. Named functions also have their own *this.* context. **Anonymous and arrow functions establish "*this.*" based on the scope the Arrow function is defined within**. It (usually) refers to the __global__ object in NodeJS, and the __window__ object in browser rendered [[javascript]].
	3. Named functions **CAN** access the `arguments[]` array variable. Anonymous cannot because there is NO Object being created.

3. **When will I use this?**
	-	[[callbacks]] are usually defined as arrow functions as they are only used once then discarded. 
	-	also `(() => console.log('Immediately Invoked'))();` immediately invoked functions are also anonymous
	
4. **When NOT to use arrow functions:**	
	1. Defining methods on an object
		- Object literal
			```js
			const calculate = {
			array: [1, 2, 3],
			sum: () => {
				console.log(this === window); // => true
				return this.array.reduce((result, item) => result + item);
				}
			};
			console.log(this === window); // => true
			// Throws "TypeError: Cannot read property 'reduce' of undefined"
			calculate.sum();
			```
			Executing `this.array` is equivalent to `window.array`, which is `undefined`.
		- Object prototype
			```js
			function MyCat(name) {
				this.catName = name;
			}
			MyCat.prototype.sayCatName = () => {
				console.log(this === window); // => true
				return this.catName;
			};
			const cat = new MyCat('Mew');
			cat.sayCatName(); // => undefined
			```
			Same as above, `this.catName` is using `window` context instead of `MyCat()`.
			
	2. Callback functions with dynamic context
		```js
		const button = document.getElementById('myButton');
		button.addEventListener('click', () => {
			console.log(this === window); // => true
			this.innerHTML = 'Clicked button';		//Will not work!
		});
		```
		`this` is `window` in an arrow function that is defined in the global context. When a click event happens, browser tries to invoke the handler function with `button` context, but arrow function does not change its pre-defined context.  `this.innerHTML` is equivalent to `window.innerHTML` and has no sense.
		
	3. Invoking constructors
		```js
		const Message = (text) => {
		this.text = text;
		};
		// Throws "TypeError: Message is not a constructor"
		const helloMessage = new Message('Hello World!');
		```
	4. Too short syntax
		```js
		const multiply = (a, b) => b === undefined ? b => a * b : a * b;
		const double = multiply(2);
		double(3); // => 6
		multiply(2, 3); // => 6
		```	

		VS the more understandable way: 
		
		```js
		function multiply(a, b) {
			if (b === undefined) {
				return function(b) {
					return a * b;
				}
		}
		return a * b;
		}
		const double = multiply(2);
		double(3); // => 6
		multiply(2, 3); // => 6
		```

---

Tags: #
Topics: [[javascript]]

