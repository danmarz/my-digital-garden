---
title: Generators
---

## Generators in [[javascript]]

Generators can appear to pause the running of a function by saving the functions **state (local memory)** *and* the **line in the execution thread** that it is currently on. 

Generators allow for restarting the previously paused function by reading the functions state and line number from the *backpack*.

---

[[javascript]]'s built in iterators are actually *objects with a next method* that when called returns the next element from the 'stream'/flow:

```js
function createFlow(array){
	let i = 0;									// i into the backpack
	const inner = { next :
		function(){
			const element = array[i];
			i++;
			return element
		}
	}
	return inner;
}

const returnNextElement = createFlow([4,5,6]);  //[4,5,6] into the backpack
const element1 = returnNextElement.next(); //returns an OBJECT { value: 4 }
const element2 = returnNextElement.next(); //returns an OBJECT { value: 5 }
```

Once we start thinking of our data as flows🌊 (where we can pick off an element one-by-one) we can rethink how we produce those flows - JavaScript now lets us produce the flows using a function 😲

```js
function *createFlow(){
	yield 4
	yield 5
	yield 6
}

const returnNextElement = createFlow()		// returns a generator OBJECT with a next property { next: createFlow() } function 
const element1 = returnNextElement.next()	// yields {value:4, done:false}->true
const element2 = returnNextElement.next()	// yields {value:5, done:false}->true
											// yields {value:6, done:false}->true
											// yields {value:undefined,done:true} 
```

Or, a more elaborate flow:
```js
function *createFlow(){
	const num = 10;
	const newNum = yield num;	// yield 10, then *pause*
	yield 5 + newNum;			// insert 2 in newNum (instead of yield above)->7
	yield 6;
}

const returnNextElement = createFlow();
const element1 = returnNextElement.next();	// 10
const element2 = returnNextElement.next(2);	// 7
```

So most importantly, for the first time we get to pause ('suspend') a function being run and then return to it by caling `returnNextElement.next()`

In asynchronous javascript we want to:
1. Initiate a task that takes a long time (e.g. requesting data from the server (api))
2. Move on to more synchronous regular code in the meantime
3. Run some functionality once the requested data has come back

> What if we were to `yield` out of the function at the moment of sending off the long-time task and return to the function only when the task is complete?

We can use the ability to pause `createFlow`s running and then restart it only when our data returns:

```js
function doWhenDataReceived (value) {
	returnNextElement.next(value)
}

function* createFlow(){
	const data = yield fetch('http://twitter.com/will/tweets/1');
	console.log(data);
}

const returnNextElement = createFlow();
const futureData = returnNextElement.next();

futureData.then(doWhenDataReceived)
```

We get to control when we return back to `createFlow` and continue executing ― by setting up the trigger to do so (`returnNetElement.next()`) to be run by our function that was triggered by the promise (`futureData`) resolution (`.then`) (when the value returned from twitter).

## [[Async - Await]] simplifies all this and finally fixes the inversion of control problem of callbacks

```js
async function createFlow(){
	console.log("Me first");
	const data = await fetch('https://twitter.com/will/tweets/1');
	console.log(data);
}

createFlow();

console.log("Me second");
```

No need for a triggered function (`.next()`) on the promise resolution, instead we auto  trigger the resumption of the `createFlow` execution (this functionality is still added to the [[Microtask queue]] though).
