---
title: Closure
---

## Closure
 
 Closure is the most esoteric of JavaScript concepts. NORMALLY *functions get a new local memory* every run/invocation and where you define your functions determines what data it has access to when you call it.

After the function finished executing, all its local memory gets deleted (*garbage collected*) EXCEPT the *returned value*. 
 
 By tweaking the way JS works under-the-hood, JS developers enabled function definitions to have an associated cache/persistent memory. 
 
 Closure is also called: 
 - C.O.V.E -> Closed Over Variable Environment
 - P.L.S.R.D. -> Persistent Lexic Scope/State Referenced Data
 - Backpack 🎒

This is the most powerful yet challenging concept in JS. It basically means that when a function is *returned* from ANOTHER function, it snapshots and carries it's environment state with it (i.e. the backpack 🎒) which means the function can access a *hidden* VE with the variables it controls AND THIS PERSISTS through multiple calls! 

Closure enables:
- Professional-grade functions like *[[once]]* and *[[memoization|memoize]]*
- Node's module pattern (the CommonJS pattern)
- Functional Programming (techniques like partial application, currying & monads)
- Building [[iterators]]
- **Asynchronous Javascript** and the *callback* pattern. Maintains state in an asynchronous world