---
Title: Rest syntax
---

Rest syntax looks exactly like [[spread syntax]]. In a way, rest syntax is the opposite of spread syntax. Spread syntax "expands" an array into its elements, while rest syntax collects multiple elements and "condenses" them into a single element.

1. **How can I use this?**
```js
	function sum(...theArgs) {
	  return theArgs.reduce((previous, current) => {
		return previous + current;
	  });
	}

	console.log(sum(1, 2, 3));
	// expected output: 6

	console.log(sum(1, 2, 3, 4));
	// expected output: 10
```	
A function definition's last parameter can be prefixed with "`...`" (three U+002E FULL STOP characters), which will cause all remaining (user supplied) parameters to be placed within a standard JavaScript array. Only the last parameter in a function definition can be a rest parameter.
	
```js
	function myFun(a,  b, ...manyMoreArgs) {
	  console.log("a", a)
	  console.log("b", b)
	  console.log("manyMoreArgs", manyMoreArgs)
	}

	myFun("one", "two", "three", "four", "five", "six")

	// Console Output:
	// a, one
	// b, two
	// manyMoreArgs, ["three", "four", "five", "six"]
```

Using rest parameters:
```js
function myFun(a, b, ...manyMoreArgs) {
  console.log("a", a)
  console.log("b", b)
  console.log("manyMoreArgs", manyMoreArgs)
}

myFun("one", "two", "three", "four", "five", "six")

// a, "one"
// b, "two"
// manyMoreArgs, ["three", "four", "five", "six"] <-- notice it's an array
```
Below, even though there is just one value, the last argument still gets put into an array:
```js
// using the same function definition from example above

myFun("one", "two", "three")

// a, "one"
// b, "two"
// manyMoreArgs, ["three"] <-- notice it's an array, even though there's just one value
```
Below, the third argument isn't provided, but `manyMoreArgs` is still an array (albeit an empty one):
```js
// using the same function definition from example above

myFun("one", "two")

// a, "one"
// b, "two"
// manyMoreArgs, [] <-- yip, still an array
```

2. **Why must I use this?**
	The **rest parameter** syntax allows a function to accept an indefinite number of arguments as an array, providing a way to represent variadic functions in JavaScript.
	
	```js
	function f(a, b, ...theArgs) {
	  // ...
	}
	```
	
	```js
	// Before rest parameters, "arguments" could be converted to a normal array using:

	function f(a, b) {
	  let normalArray = Array.prototype.slice.call(arguments)
	  // -- or --
	  let normalArray = [].slice.call(arguments)
	  // -- or --
	  let normalArray = Array.from(arguments)

	  let first = normalArray.shift()  // OK, gives the first argument
	  let first = arguments.shift()    // ERROR (arguments is not a normal array)
	}

	// Now, you can easily gain access to a normal array using a rest parameter

	function f(...args) {
	  let normalArray = args
	  let first = normalArray.shift() // OK, gives the first argument
	}
	```

3. **When will I use this?**
	The difference between rest parameters and the [[arguments object]]:
		- The `arguments` object is **not a real array**, while rest parameters are `Array` instances, meaning methods like `sort`, `map`, `forEach` or `pop` can be applied on it directly. `.length` is a shared by both 'arrays'.
		- The `arguments` object has additional functionality specific to itself (like the `calee` property)
		- The `...restParam` bundles all the extra parameters into a single array, therefore it does not contain any named argument defined **before** the `...restParam`. Whereas the `arguments` object contains all of the parameters -- including all of the stuff in the `...restParam` -- **un**bundled.

Rest parameters are real arrays; the arguments object is not.
```js
function sortRestArgs(...theArgs) {
  let sortedArgs = theArgs.sort()
  return sortedArgs
}

console.log(sortRestArgs(5, 3, 7, 1)) // 1, 3, 5, 7

function sortArguments() {
  let sortedArgs = arguments.sort()
  return sortedArgs  // this will never happen
}

console.log(sortArguments(5, 3, 7, 1))
// throws a TypeError (arguments.sort is not a function)
```

Using rest parameters in combination with ordinary parameters
```js
function multiply(multiplier, ...theArgs) {
  return theArgs.map(element => {
    return multiplier * element
  })
}

let arr = multiply(2, 15, 25, 42)
console.log(arr)  // [30, 50, 84]
```
# ---

Tags: #
Topics: [[javascript]] [[spread syntax]] [[arguments object]]

