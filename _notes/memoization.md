---
title: Memoization
---

## 'memoize' (and '[[once]]') are professional helper functions built on top of [[javascript|JS]]'s [[closure]] functionality

Memoize *gives functions persistent memories of their previous input/output combinations.*

An example would be having a function like `nthPrime(127001)`,  getting to the 127.001st prime number is a heavy computational task, so it will take a while to complete. 

By declaring the function within another function we can store that Prime number in a const INSIDE the function's backpack (as key: value) and later instantly serve the same call without having to re-do the calculation OR pollute the global() context with innecessary arrays.

