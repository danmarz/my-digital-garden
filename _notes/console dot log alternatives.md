---
title: console.log() alternatives
---

Sometimes while debugging you may use `console.log` or maybe `console.warn` too. But there are a lot more methods which can help you debug your code even better. Let's take a look at some of them:

## `console.table()`

The handiest method on this list. Can be used to log any object or array in table form.

```
console.table([
  {
    "userId": 1,
    "id": 1,
    "title": "delectus aut autem",
    "completed": false
  },
  {
    "userId": 1,
    "id": 2,
    "title": "quis ut nam facilis et officia qui",
    "completed": false
  },
  {
    "userId": 1,
    "id": 3,
    "title": "fugiat veniam minus",
    "completed": false
  },
  {
    "userId": 1,
    "id": 4,
    "title": "et porro tempora",
    "completed": true
  },
  {
    "userId": 1,
    "id": 5,
    "title": "laboriosam mollitia et enim quasi adipisci quia provident illum",
    "completed": false
  },
  {
    "userId": 1,
    "id": 6,
    "title": "qui ullam ratione quibusdam voluptatem quia omnis",
    "completed": false
  },
]);
```

This will give us a neat little table:

![console.table()](https://cdn.hashnode.com/res/hashnode/image/upload/v1635608677908/KwgQ9EgZH.png?auto=compress,format&format=webp)

Cool?

## `console.assert()`

`console.assert()` is used to assert that something is truthy. If not, it will log a message to the console.

```
const isEven = n => n % 2 === 0;

for (let i = 0; i < 3; i++) {
    console.assert(isEven(i), '%s is not even!', i);
}
```

This will log `Assertion failed: 1 is not even!` because well, one is not even! (Who told you that one is even?? Go to school and learn a thing or two)

## `console.count()`

`console.count()` is used to check how many times this line has been called.

```
for (let i = 0; i < 3; i++) {
    console.count();
}
```

This will log:

```
default: 1
default: 2
default: 3
```

You can also label the count:

```
for (let i = 0; i < 3; i++) {
    console.count('outer loop');
    for (let i = 0; i < 3; i++) {
        console.count('inner loop');
    }
}
```

This will log:

```
outer loop: 1
inner loop: 1
inner loop: 2
inner loop: 3
outer loop: 2
inner loop: 4
inner loop: 5
inner loop: 6
outer loop: 3
inner loop: 7
inner loop: 8
inner loop: 9
```

## `console.group()` and `console.groupEnd()`

`console.group()` and `console.groupEnd()` are used for grouping similar (or different ;) logs together.

```
console.group('group 1');
for (let i = 0; i < 3; i++) {
    console.log(i);
}
console.groupEnd('group 1');

console.group('group 2');
for (let i = 0; i < 3; i++) {
    console.log(i);
}
console.groupEnd('group 2');
```

That should log two openable/closeable groups which can be handy when dealing with a lot of logs.

Inside the groups you can use any other console methods, even nested `console.group()`

You can also use `console.groupCollapsed()` to make the groups closed by default.

## `console.time()` and friends

You can use `console.time()` and it's friends `console.timeStart()`, `console.timeEnd()`, and `console.timeLog()` to measure stuff.

```
console.time();

for (let i = 0; i < 1e9; i++) {
  
}

console.timeEnd()
```

This will log something like:

```
default: 9531ms - timer ended
```

`9531ms` is the time between `console.time()` and `console.timeEnd()`.

You can also label these timers so you can have multiple independent timers running at the same time:

```
console.time('first');

for (let i = 0; i < 1e9; i++) {
  
}

console.timeLog('first'); 

console.time('second');

for (let i = 0; i < 1e9; i++) {
  
}

console.timeEnd('first');
console.timeEnd('second');
```

This will log:

```
first: 8497ms
first: 17815ms - timer ended
second: 9318ms - timer ended
```

## `console.trace()`

When you are working with a lot of nested function calls or recursion at some point you will need to know which function called who. `console.trace()` is a handy way to do that:

```
const shallow = () => deep();
const deep = () => deeper();
const deeper = () => deepest();
const deepest = () => console.trace()

shallow()
```

This will log this stacktrace:

```
console.trace()
    deepest     debugger eval code:4
    deeper      debugger eval code:3
    deep        debugger eval code:2
    shallow     debugger eval code:1
    <anonymous> debugger eval code:1
```

Now we can easily see that shallow called `deep`, which called `deeper` which called `deepest`

___

That's the end of the list!

Topics: [[javascript]]