---
Title: What is the difference between `===` and `==`
---

# What is the difference between `===` and `==`

JavaScript is a loosely typed language which means we don't have to declare a variable's type at creation, and a variable can change types throughout the lifetime of the program. Because of this loose typing, JavaScript introduced two ways to compare variables with `===` and `==`.

To a computer, the characters `2` and `"2"` are entirely different. In JavaScript the first is considered a number type and the second is a string type.

Using the `===` operator, we are trying to determine if two items are exactly equal or "strictly equal", which means they must match in type and value.

If we instead use `==`, JavaScript will try to coerce the values (convert them to the same type) and then compare them. The intention here is that we only care if the values are similar, even if they are not originally the same type, also called "loosely equal".

```javascript
2 === 2 // true
2 === "2" // false

2 == 2 // true
2 == "2" // true
```

For `2 == "2"`, JavaScript converts the `"2"` to a number `2` before checking if they are equal which is why we get `true`.

The `===` comparison is almost always favored over `==` because it is more explicit and prevents unintended bugs. If you need to make comparisons in your interview code, I would highly recommend to never use `==` or be prepared to make a strong argument for why you did.

The following are some of the weird cases where JavaScript tries to convert values and compare them:

```javascript
0 == false; // true
0 == ''; // true
0 == '0'; // true
1 == '1'; // true
1 == [1]; // true
1 == true; // true
null == undefined; // true
```

These would all return `false` if the `===` operator was used.
# ---

Tags: #coding #interview

Topics: [[javascript]] 

