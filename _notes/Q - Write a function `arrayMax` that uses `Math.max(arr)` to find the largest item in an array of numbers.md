---
Title: Write a function `arrayMax` that uses `Math.max(arr)` to find the largest item in an array of numbers
---

# Write a function `arrayMax` that uses `Math.max(arr)` to find the largest item in an array of numbers

The `Math.max` method takes an arbitrarily long list of parameters and returns the largest.

```javascript
Math.max(2, 4, 10, 20, 100); // 100
```

We instead want it to work with an array.

```javascript
const nums = [2, 4, 10, 20, 100];
Math.max(nums); // NaN
```

The above returns `NaN`, so we'll write our own function `arrayMax` that still uses `Math.max` to find the largest number in an array.

The simplest solution is the spread operator `...`. It will decompose an array into its individual items.

```javascript
function arrayMax(arr) {
  return Math.max(...arr);
}
arrayMax([2, 4, 10, 20, 100]); // 100
```

This question has historically been used to test an understanding of `apply`. It will pass an array as individual items to a function. `Math.max` is also a static method which means it is called without instantiating an object and does not require access to `this`, so we pass `null` as the first argument.

```javascript
function arrayMax(arr) {
  return Math.max.apply(null, arr);
}
arrayMax([2, 4, 10, 20, 100]); // 100
```

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

