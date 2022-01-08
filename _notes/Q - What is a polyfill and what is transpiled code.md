---
Title: What is a polyfill and what is transpiled code
---

# What is a polyfill and what is transpiled code

Since ES2015, new features have been routinely added to the JavaScript specification. Unfortunately not all browsers implement the new features, or our users are still using old browsers. To allow developers the ability to use the new features and be compatible with all browsers, we use polyfills and/or transpile our code.

A polyfill is a piece of code that implements new JavaScript features in older ES5 JavaScript syntax that is compatible with more browsers. For example, we can use a polyfill for the `Promise` object to use promises in any browser.

Another option to use new features and syntax is to transpile code. Webpack or another bundler takes our modern code and transforms it into a version that all browsers can read. For example, if you tried to use the spread operator `...`, it would throw an error in an old browser.

# ---

Tags: #coding #interview

Topics: [[javascript]] 


