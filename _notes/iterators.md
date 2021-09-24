---
title: Iterators
---

## Iterators in [[javascript]]

Most of the time using a `for` loop is a pain in the a\*\*. Almost always we get data in an array with just an `array[position i]` to navigate instead of a simple `array.nextElement()`, and everytime we leave the function's *execution environment*, we have to begin anew.

Iterators enable the `.next()` functionality via [[closure]] in JS, by storing the current array position in the function's *backpack*.