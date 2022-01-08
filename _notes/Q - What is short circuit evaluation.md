---
Title: What is short circuit evaluation (`||`, `&&`, `??`)
---

# What is short circuit evaluation (`||`, `&&`, `??`)

Short circuit evaluation is a technique where you use binary logical operators to avoid unnecessary work. It can also be used to assign variables based on the [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) or [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) values used.

A truthy value means that the value is coerced to `true`, and a value is falsy if its value is coerced to `false` (for example, an empty string `''`).

Binary logical operators will look something like the following:

```javascript
item1 || item2
item1 && item2
item1 ?? item2
```

The OR `||` operator will return `item1` if it is truthy, otherwise it will return `item2`. We use the `||` to short circuit by being able to skip to the second item if the first is true.

```javascript
true || true         returns true
true || false        returns true
false || true        returns true
false || false       returns false

// Short circuit with ||
// Will only do the expensive request if the user doesn't exist in cache
const user = cache.user || getUserFromExpensiveRequest();
```

The AND `&&` operator will return `item1` if it is falsy, otherwise it will return `item2`. We use the `&&` to short circuit by being able to skip the second item if the first is false. This is also a common pattern for conditional rendering in web applications.

```javascript
true && true         returns true
true && false        returns false
false && true        returns false
false && false       returns false

// Short circuit with &&
// Will only perform the expensive calculation if you are logged in
isLoggedIn && doExpensiveCalculationIfLoggedIn();

// Conditional rendering
const canSeeComponent = true;

<div>
  {canSeeComponent && <RenderThisConditionally />}
</div>
```

The nullish coalescing operator `??` was released in ES2020. It is similar to `||` but instead of considering all falsy values, it only yields to `item2` if `item1` is either `null` or `undefined`.

```javascript
null ?? true         returns true
undefined ?? true    returns true
false ?? true        returns false
```
# ---

Tags: #coding #interview

Topics: [[javascript]] 

