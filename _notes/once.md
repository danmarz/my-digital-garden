---
title: Once
---

## 'once', like '[[memoization|memoize]]' are professional helper functions built on top of [[javascript|JS]]'s [[closure]] functionality

'Once' function allows us to control the execution of a function via an internal, hidden, unaccessible from another context ―  counter stored inside the functions *[[closure|backpack]]*. i.e.:
```
if (counter > 0) return "f not allowed to run more than ONCE";
	else //do the thing;```