---
Title: Explain JavaScript strict mode
---

# Explain JavaScript strict mode

Strict mode was added in ES5 as an optional opt-in feature. It is enabled by adding the string `'use strict;'` to the top of a file or function. Strict mode is meant to make our code safer and more robust. ES2015 modules (code using the `import`/`export` syntax) are automatically in strict mode.

The overall goal is to make JavaScript more secure, efficient, and prepared for future updates.

The advantages of strict mode are:

-   Eliminates some silent JS errors by `throw`ing them instead
-   Removes the ability to assign a variable without being declared (previously assigning a variable without `var` just became a global variable)
-   Makes JS more secure about how it handles `this`
-   Function parameter names must be unique
-   Simplifies the usage of `eval` and `arguments`, which allows for optimizations
-   Removes the ability to use features that are generally regarded as "bad"
-   Paves the way for new JavaScript features by disallowing future keywords such as `private` and `interface`

# ---

Tags: #coding #interview

Topics: [[javascript]] 


