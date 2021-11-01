---
Title: Logical nullish assignment (??=)
---

1. **How can I use this?**
	-	`expr1 ??= expr2`
	```js
	const a = { duration: 50 };

	a.duration ??= 10;
	console.log(a.duration);
	// expected output: 50

	a.speed ??= 25;
	console.log(a.speed);
	// expected output: 25
	```

2. **Why must I use this?**

The logical nullish assignment (`x ??= y`) operator only assigns if `x` is [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) (`null` or `undefined`).

## Short-circuit evaluation

The [nullish coalescing](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) operator is evaluated left to right, it is tested for possible short-circuit evaluation using the following rule:

`(some expression that is neither null nor undefined) ?? expr` is short-circuit evaluated to the left-hand side expression if the left-hand side proves to be neither `null` nor `undefined`.

Short circuit means that the `expr` part above is **not evaluated**, hence any side effects of doing so do not take effect (e.g., if `expr` is a function call, the calling never takes place).

Logical nullish assignment short-circuits as well meaning that `x ??= y` is equivalent to:
```js
x ?? (x = y);
```
And not equivalent to the following which would always perform an assignment:
```js
x = x ?? y;
```

3. **When will I use this?**
	-	

# ---

Tags: #
Topics: [[javascript]]

