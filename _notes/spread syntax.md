---
Title: Spread syntax
---

**Spread syntax** (`...`) allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected.

1. **How can I use this?**
	```js
	function sum(x, y, z) {
	  return x + y + z;
	}

	const numbers = [1, 2, 3];

	console.log(sum(...numbers));
	// expected output: 6

	console.log(sum.apply(null, numbers));
	// expected output: 6
	```
	

1. **Why must I use this?**
	Spread syntax can be used when all elements from an object or array need to be included in a list of some kind.

3. **When will I use this?**
		For function calls:
	```js
	myFunction(...iterableObj); 
	// pass all elements of iterableObj as arguments to function myFunction
	```
	
	For array literals or strings:
	```js
	[...iterableObj, 4, 'five']
	// combine two arrays by inserting all elements from iterableObj
	```
	
	For a more powerful array literal:
	```js
	let parts = ['shoulders', 'knees'];
	let lyrics = ['head', ...parts, 'and', 'toes'];
	//  ["head", "shoulders", "knees", "and", "toes"]
	```
	
	For object literals (new in ECMAScript 2018):
	```js
	let objClone = { ...obj }; 
	// pass all key:value pairs from object obj
	```
	
	To copy an array:
	```js
	let arr = [1, 2, 3];
	let arr2 = [...arr]; // like arr.slice()

	arr2.push(4);
	//  arr2 becomes [1, 2, 3, 4]
	//  arr remains unaffected
	```
	
	To concatenate an array:
	```js
	let arr1 = [0, 1, 2];
	let arr2 = [3, 4, 5];

	arr1 = [...arr1, ...arr2];
	//  arr1 is now [0, 1, 2, 3, 4, 5]
	// Note: Not to use const otherwise, it will give TypeError (invalid assignment)
	// same as arr1.concat(arr2);
	```
	
	To prepend items to an array (`unshift()`):
	```js
	arr1 = [...arr2, ...arr1];
	//  arr1 is now [3, 4, 5, 0, 1, 2]
	```
	
	To replace `apply()` in cases where you want to use the elements of an array as arguments to a function:
	```js
	function myFunction(x, y, z) { }
	let args = [0, 1, 2];
	myFunction(...args);
	
	// another example using the constructor new
	
	let dateFields = [1970, 0, 1];  // 1 Jan 1970
	let d = new Date(...dateFields);
	```

# ---

Tags: #
Topics: [[javascript]] [[rest syntax]]

