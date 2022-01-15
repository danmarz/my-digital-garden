---
Title: Explain boolean type coercion and what it means for a value to be truthy or falsy
---

# Explain boolean type coercion and what it means for a value to be truthy or falsy

Since JavaScript is a loosely typed language, we can use variables of any type and JavaScript will try to coerce them (transform their type) to whatever is needed in a particular situation. For example, an `if (conditional) {...}` statement is expecting a boolean, but you can pass any variable type to it, and it will still work.

```javascript
const str = 'hello';
const emptyStr = '';

if (str) { console.log('this shows up'); }
if (emptyStr) { console.log('this will not log'); }
```

What is happening is that JavaScript is converting the string above to a boolean. A string containing characters is converted to `true` and an empty string is converted to `false`. Then the `if` statement handles the resulting coercion.

Every variable type is converted in this manner. The values that convert to `true` are considered "truthy". The values that convert to `false` are considered "falsy".

Below is a list of all the falsy values in JavaScript. Everything else is considered truthy.

```javascript
false
null
undefined
"" // empty string
NaN
0 // also -0 and 0n
```

It's worth noting that empty arrays `[]` and empty objects `{}` are considered truthy.

# ---

Tags: #coding #interview

Topics: [[Interview Qs]]

