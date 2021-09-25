---
Title: Callback functions in JS
---

## Callback functions in JS

Callback functions are functions passed as input(arguments) to other functions. => baby 🐣 functions

```js
function doWork(function_to_do, times_to_do) {

	let result = function_to_do * times_to_do
	return result;
}

multiplyByTwo(input_num) {
	return input_num * 2;
}

doWork(multiplyByTwo(8), 3);

```