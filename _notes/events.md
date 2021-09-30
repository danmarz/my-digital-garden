---
Title: Event emitter
---

# The Node.js Event emitter

On the backend side, Node.js offers us the option to use the `events` module which offers the `EventEmitter` class, which we'll use to handle our events.

```js
import EventEmitter from 'events'
const eventEmitter = new EventEmitter();
```

This object exposes, among many others, the `on` and `emit` methods.
-   `on` is used to add a callback function that's going to be executed when the event is triggered
```js
eventEmitter.on('start', () => {
  console.log('started')
})
```
-   `emit` is used to trigger an event
```js
eventEmitter.emit('start')
```

You can pass arguments to the event handler by passing them as additional arguments to `emit()`:

```js
eventEmitter.on('start', number => {
  console.log(`started ${number}`)
})

eventEmitter.emit('start', 23) // started 23
```

Multiple arguments:
```js
eventEmitter.on('start', (start, end) => {
  console.log(`started from ${start} to ${end}`)
})

eventEmitter.emit('start', 1, 100)
```

The EventEmitter object also exposes several other methods to interact with events, like

-   `once()`: add a one-time listener
-   `removeListener()` / `off()`: remove an event listener from an event
-   `removeAllListeners()`: remove all listeners for an event