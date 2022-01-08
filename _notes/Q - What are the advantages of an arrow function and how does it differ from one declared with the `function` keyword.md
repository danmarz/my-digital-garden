---
Title: What are the advantages of an arrow function `=>` and how does it differ from one declared with the `function` keyword
---

# What are the advantages of an arrow function `=>` and how does it differ from one declared with the `function` keyword

Arrow functions offer a few advantages:

1.  A more concise and compact syntax. `() => {}` vs `function() {}`.
    
2.  Implicit return if the function is one line. You can leave out the function body and JavaScript will automatically return the value.
    

```javascript
const double = (num) => 2 * num;
// Equivalent to
const double = function(num) {
  return 2 * num;
}
```

3.  A key benefit is how arrow functions handle `this`. Previously, using `function` bound the value of `this` based on where the function was called. This forced developers to add hacky fixes to retain the original `this` value. On the other hand, an arrow function does not create its own `this` and instead uses the value from its enclosing scope.

Other notable differences include: arrow functions don't have access to the `arguments` object, arrow functions don't change `this` from `bind`/`call`/`apply` since they don't define their own value for it, and arrow functions can't be used as constructors functions with the `new` keyword.

# ---

Tags: #coding #interview

Topics: [[javascript]] 

