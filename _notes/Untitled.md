---
title: Generators
---

## Generators in [[javascript]]

Generators can appear to pause the running of a function by saving the functions **state (local memory)** *and* the **line in the execution thread** that it is currently on. 

Generators allow for restarting the previously paused function by reading the functions state and line number from the *backpack*.