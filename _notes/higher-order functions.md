---
Title: Higher-order functions (HOFs) in JS
---

# Higher-order functions (HOFs) in JS

The concept of HOFs is synonymous with [[callbacks]] in [[javascript]]. 

A callback is a simple function that's passed as a value to another function. For example:
It's common to wrap all your client code in a `load` event listener on the `window` object, which runs the callback function only when the page is ready:

```js
window.addEventListener('load', () => {

 //window loaded
 //do what you want

})
```

Another example of callbacks but not using DOM event driven is when using timers:
```js
setTimeout(() => {
  // runs after 2 seconds
}, 2000)
```


1. **How can I use this?**
	-	A HOF has a minimum of one (but usually both) of the following features:
		1. Accepts a function as an argument
		2. Returns a funcion
		
2. **Why must I use this?**
	- Enables maintaining of state by means of [[closure]] in JS.

3. **When will I use this?**
	-	For example, we can build a higher order function , *withCount()*, that modifies **ANY FUNCTIoN** passed to it so that it logs out how many times it has been called:
```js
const withCount = fn => {
	let count = 0
	
	return (...args) => {
		console.log(`Current count: ${count++}`)
		return fn(...args)
	}
}

const add = (x, y) => x +y

const countedAdd = withCount(add)

console.log(countedAdd(1, 2));	// Call count: 1
console.log(countedAdd(2, 2));	// Call count: 2
console.log(countedAdd(3, 2));	// Call count: 3

const countedAdd2 = countedAdd;	// PASS BY REFERENCE

console.log(countedAdd2(1, 2));	// Call count: 4
console.log(countedAdd2(2, 2));	// Call count: 5
console.log(countedAdd2(3, 2));	// Call count: 6

const countedAdd3 = withCount(add);	// NEW ASSIGNMENT
console.log(countedAdd3(1, 2));	// Call count: 1
console.log(countedAdd3(2, 2));	// Call count: 2
console.log(countedAdd3(3, 2));	// Call count: 3
```

# ---

Tags: #source/book📚 #quotes 
Topics: [\[\Einstein\]\] [\[\science\]\] [\[\creativity\]\]

