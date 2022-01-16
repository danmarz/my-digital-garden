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

Then we can have add a `test()` function:

```js
// refactor tests so that each gets a title and we can easily track which one failed
function test(title, callback) {
 // wrap in try/catch so tests can continue even if one throws an error
 try {
	 callback()
 } catch (error) {
	 console.error(error)
 }
}
```

So now we have this as our test file:
```js
// both tested functions are broken now -- and now both tests are being run!
const sum = (a, b) => a - b
const subtract = (a, b) => a + b

test("sum adds numbers", () => {
 const result = sum(7, 3)
 const expected = 10
 expect(result).toBe(expected)
})

test("subtract subtracts numbers", () => {
 const result = subtract(7, 3)
 const expected = 4
 expect(result).toBe(expected)
})

function test(title, callback) {
 try {
	 callback()
	 console.log(`✔ ${title} passed.`)
 } catch (error) {
	 console.error(`❌ ${title} failed.`)
	 console.error(error)
 }
}

function expect(result) {
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

