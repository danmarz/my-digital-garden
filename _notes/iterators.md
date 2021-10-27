---
title: Iterators
---

## Iterators in [[javascript]]

Programs store data and apply functionality to it. But there are two parts to applying functions to **collections, arrays or object** data:

1. The process of accessing each element
2. What we want to do to each element

For 1, most of the time using a `for` loop is a pain in the a\*\*. Almost always we get data in an array with just an `array[position i]` to navigate instead of a simple `array.nextElement()`, and everytime we leave the function's *execution environment*, we have to begin anew.

**BUT what if we start by returning a function from another function?**

Iterators enable the `.next()` functionality via [[closure]] in JS, by storing the current array position in the function's *backpack*.

```js
function createFunction(array){
	let i = 0;										// i into the backpack
	function inner(){
		const element = array[i];
		i++;
		return element
	}
	return inner;
}

const returnNextElement = createFunction([4,5,6]);  //[4,5,6] into the backpack
const element1 = returnNextElement(); //returns 4
const element2 = returnNextElement(); //returns 5
```

For .. of .. loops => use `for...of` for *==values==*
```js
for (element of array) {
	console.log( element );
}
```
`for..of` works of objects that implement the `Symbol.iterator` property allowing access to stored values like 'Map' and 'Set' built-in objects.

---

For .. in .. loops => `for..in..keys` === foreign keys === use `for...in` for *==keys==*. 

```js
for (let _property_ in _object_) {
	console.log( _property_ + ":" + _object_[_property_] );
}
```
`for..in` operates on any object; it serves as a way to inspect properties on this object.
Also `in` gives you ==index==. This is more than enough to remember the difference.

