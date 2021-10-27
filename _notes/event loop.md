---
Title: JS Event loop
---

## JS Event loop

The event loop is the feature in [[javascript]] that checks before each line of code executed if the call stack still has global code to run and if there is something in the *callback queue*.

If the *call stack* isn't empty, it's not going to even bother to check the *call queue*, but if the call stack is empty, it pulls the first function from the callback queue and executes it.