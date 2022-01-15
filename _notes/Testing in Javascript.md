---
Title: Testing in Javascript
---

## What a testing framework is under-the-hood

```js
// sum is intentionally broken so you can see errors in the tests
const sum = (a, b) => a - b
const subtract = (a, b) => a - b

let result, expected

result = sum(7, 3)
expected = 10
// if (result !== expected) {
//   console.error(`The expected value was ${expected} but we got ${result}`)
// }
expect(result).toBe(expected)

result = subtract(7, 3)
expected = 4
// if (result !== expected) {
//   console.error(`The expected value was ${expected} but we got ${result}`)
// }
expect(result).toBe(expected)

// refactor the if statement so it becomes reusable - this is what testing frameworks do!
function expect(result) {
 // return an object { } that has the toBe() function defined
 return {
	 toBe(expected) {
 if (result !== expected) {
	 throw new Error(`${result} is not equal to ${expected}`)
		 }
	 },
 }
}
```

# ---

Tags: #testing 

Topics: [[javascript]] 

