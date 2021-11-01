---
Title: Nullish coalescing operator (??)
---


1. **How can I use this?**

Basic syntax:
```js
leftExpr ?? rightExpr
```
Example:
```js
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

2. **Why must I use this?**

The **nullish coalescing operator (`??`)** is a logical operator that returns its right-hand side operand when its left-hand side operand is [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null) or [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined), and otherwise returns its left-hand side operand.

This can be contrasted with the [logical OR (`||`) operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR), which returns the right-hand side operand if the left operand is _any_ [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) value, not only `null` or `undefined`. In other words, if you use `||` to provide some default value to another variable `foo`, you may encounter unexpected behaviors if you consider some falsy values as usable (e.g., `''` or `0`). See below for more examples.

The nullish coalescing operator has the fifth-lowest [operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence), directly lower than `||` and directly higher than the [conditional (ternary) operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

3. **When will I use this?**

When we want to provide default values, but keep existing values which are not `null` or `undefined`.

```js
const nullValue = null;
const emptyText = ""; // falsy
const someNumber = 42;

const valA = nullValue ?? "default for A";
const valB = emptyText ?? "default for B";
const valC = someNumber ?? 0;

console.log(valA); // "default for A"
console.log(valB); // "" (as the empty string is not null or undefined)
console.log(valC); // 42
```

Earlier, when one wanted to assign a default value to a variable, a common pattern was to use the logical OR operator ([`||`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR)), however the way the `||` operator works is by being a boolean operator using coercion on the left hand-side operand and evaluating any *falsy* value (`0`, `""`, `NaN`, `null`, `undefined`) to false. This was causing problems in case you were wanting to use `0`, an empty string `""` or `NaN` as valid values.

>The nullish coalescing operator avoids this by only returning the second operand when the first one __strictly__ evaluates to eiether `null` or `undefined` (but no other falsy values)!

```js
let myText = ''; // An empty string (which is also a falsy value)

let notFalsyText = myText || 'Hello world';
console.log(notFalsyText); // Hello world

let preservingFalsy = myText ?? 'Hi neighborhood';
console.log(preservingFalsy); // '' (as myText is neither undefined nor null)
```

## Short-circuiting
Like the OR and AND operators, the right-hand side expression is not evaluated if the left-hand side proves to be neither `null` nor `undefined`.

```js
function A() { console.log('A was called'); return undefined;}
function B() { console.log('B was called'); return false;}
function C() { console.log('C was called'); return "foo";}

console.log( A() ?? C() );
// logs "A was called" then "C was called" and then "foo"
// as A() returned undefined so both expressions are evaluated

console.log( B() ?? C() );
// logs "B was called" then "false"
// as B() returned false (and not null or undefined), the right
// hand side expression was not evaluated
```

## No chaining with AND and OR operators
It is not possible to combine both the AND (`&&`) and OR operators (`||`) directly with `??`. A [`SyntaxError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) will be thrown in such cases.

```js
null || undefined ?? "foo"; // raises a SyntaxError
true || undefined ?? "foo"; // raises a SyntaxError
```

However, providing parenthesis to explicitly indicate precedence is allowed:
```js
(null || undefined) ?? "foo"; // returns "foo"
```

## Relationship with the optional chaining operator (?.)
The nullish coalescing operator treats `undefined` and `null` as specific values and so does the [[Optional chaining operator]] (?.) which is useful to access a property of an object which my be `null` or `undefined`

```js
let foo = { someFooProp: "hi" };

console.log(foo.someFooProp?.toUpperCase() ?? "not available"); // "HI"
console.log(foo.someBarProp?.toUpperCase() ?? "not available"); // "not available"
```

# ---

Tags: #operators
Topics: [[javascript]] [[Logical nullish assignment]]

