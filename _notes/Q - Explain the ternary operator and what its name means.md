---
Title: Explain the ternary operator and what its name means
---

# Explain the ternary operator and what its name means

A ternary operator means there are three operands: `1 ? 2 : 3`

It is similar to an if-else statement: `condition ? ifTrue : elseFalse`. One difference to note is that inside an if-else statement, you execute code, but with a ternary operator, it returns either the first or second value and can be assigned to a variable.

The example below performs equivalent operations:

```javascript
const drink = age >= 21 ? 'beer' : 'water';

let drink;
if (age >= 21) {
  drink = 'beer';
} else {
  drink = 'water';
}
```
# ---

Tags: #coding #interview

Topics: [[javascript]] 

