---
Title: JS ES6 Destructuring notation
---


This capability is similar to features present in languages such as [[Python]] and Perl.

1. **How can I use this?**
	```js
	let a, b, rest;
	[a, b] = [10, 20];

	console.log(a);
	// expected output: 10

	console.log(b);
	// expected output: 20

	[a, b, ...rest] = [10, 20, 30, 40, 50];

	console.log(rest);
	// expected output: Array [30,40,50]
	```
	RL example:
	`const { origin, destination } = req.query;`

	```js
	app.get('/', function(req, res){
	   const {id, since, fields, anotherField} = req.query;
	});
	```

	Destructuring works with defaults too:
	```js
	const req = {    //destructuring with provided default values!
	  query: {
		id: '123',
		fields: ['a', 'b', 'c']
	  }
	}

	const {
	  id,
	  since = new Date().toString(),
	  fields = ['x'],
	  anotherField = 'default'
	} = req.query;
	```

2. **Why must I use this?**
	The **destructuring assignment** syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

	```js
	const [firstElement, secondElement] = list;
	// is equivalent to:
	// const firstElement = list[0];
	// const secondElement = list[1];
	```

3. **When will I use this?**
	## Array destructuring
	- Basic variable assignment:
	```js
	const foo = ['one', 'two', 'three'];

	const [red, yellow, green] = foo;
	console.log(red); // "one"
	console.log(yellow); // "two"
	console.log(green); // "three"
	```
	- Asignment separate from declaration:
	```js
	let a, b;

	[a, b] = [1, 2];
	console.log(a); // 1
	console.log(b); // 2
	```
	- Default values:
	```js
	let a, b;

	[a=5, b=7] = [1];
	console.log(a); // 1
	console.log(b); // 7
	```
	- Swapping variables:
	```js
	let a = 1;
	let b = 3;

	[a, b] = [b, a];
	console.log(a); // 3
	console.log(b); // 1

	const arr = [1,2,3];
	[arr[2], arr[1]] = [arr[1], arr[2]];
	console.log(arr); // [1,3,2]
	```
	- Parsing an array returned from a function:
	```js
	function f() {
	  return [1, 2];
	}

	let a, b;
	[a, b] = f();
	console.log(a); // 1
	console.log(b); // 2
	```
	- Ignoring some returned values:
	```js
	function f() {
	  return [1, 2, 3];
	}

	const [a, , b] = f();
	console.log(a); // 1
	console.log(b); // 3

	const [c] = f();
	console.log(c); // 1
	```
	- Ignoring all returned values:
	```js
	[,,] = f();
	```
	- Assigning the rest of an array to a variable:
	```js
	const [a, ...b] = [1, 2, 3];
	console.log(a); // 1
	console.log(b); // [2, 3]
	```
	- Unpacking values from a regular expression match:
	When the regular expression `exec()` method finds a match, it returns an array containing first the entire matched portion of the string and then the portions of the string that matched each parenthesized group in the regular expression. Destructuring assignment allows you to unpack the parts out of this array easily, ignoring the full match if it is not needed.
	```js
	function parseProtocol(url) {
	  const parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
	  if (!parsedURL) {
		return false;
	  }
	  console.log(parsedURL);
	  // ["https://developer.mozilla.org/en-US/docs/Web/JavaScript", 
	  // "https", "developer.mozilla.org", "en-US/docs/Web/JavaScript"]

	  const [, protocol, fullhost, fullpath] = parsedURL;
	  return protocol;
	}

	console.log(parseProtocol('https://developer.mozilla.org/en-US/docs/Web/JavaScript'));
	// "https"
	```
	## Object destructuring
	- Basic assignement:
	```js
	const user = {
		id: 42,
		isVerified: true
	};

	const {id, isVerified} = user;

	console.log(id); // 42
	console.log(isVerified); // true
	```
	- Assignment separate from declaration:
	```js
	let a, b;

	({a, b} = {a: 1, b: 2});
	// **Note:** The parentheses `( ... )` around the assignment statement are required 
	// when using object literal destructuring assignment without a declaration.
	```
	- Nested object and array destructuring:
	```js
	const metadata = {
	  title: 'Scratchpad',
	  translations: [
		{
		  locale: 'de',
		  localization_tags: [],
		  last_edit: '2014-04-14T08:43:37',
		  url: '/de/docs/Tools/Scratchpad',
		  title: 'JavaScript-Umgebung'
		}
	  ],
	  url: '/en-US/docs/Tools/Scratchpad'
	};

	let {
	  title: englishTitle, // rename
	  translations: [
		{
		   title: localeTitle, // rename
		},
	  ],
	} = metadata;

	console.log(englishTitle); // "Scratchpad"
	console.log(localeTitle);  // "JavaScript-Umgebung"
	```
	- For ... of iteration and destructuring
	```js
	const people = [
	  {
		name: 'Mike Smith',
		family: {
		  mother: 'Jane Smith',
		  father: 'Harry Smith',
		  sister: 'Samantha Smith'
		},
		age: 35
	  },
	  {
		name: 'Tom Jones',
		family: {
		  mother: 'Norah Jones',
		  father: 'Richard Jones',
		  brother: 'Howard Jones'
		},
		age: 25
	  }
	];

	for (const {name: n, family: {father: f}} of people) {
	  console.log('Name: ' + n + ', Father: ' + f);
	}

	// "Name: Mike Smith, Father: Harry Smith"
	// "Name: Tom Jones, Father: Richard Jones"
	```
	- Invalid JavaScript identifier as a property name:
	```js
	const foo = { 'fizz-buzz': true };
	const { 'fizz-buzz': fizzBuzz } = foo;

	console.log(fizzBuzz); // "true"
	```
	- Combined Array and Object Destructuring
	Array and Object destructuring can be combined. Say you want the third element in the array `props` below, and then you want the `name` property in the object, you can do the following:
	```js
	const props = [
	  { id: 1, name: 'Fizz'},
	  { id: 2, name: 'Buzz'},
	  { id: 3, name: 'FizzBuzz'}
	];

	const [,, { name }] = props;

	console.log(name); // "FizzBuzz"
	```
	- The prototype chain is looked up when the object is deconstructed
	When deconstructing an object, if a property is not accessed in itself, it will continue to look up along the prototype chain.
	```js
	let obj = {self: '123'};
	obj.__proto__.prot = '456';
	const {self, prot} = obj;
	// self "123"
	// prot "456" (Access to the prototype chain)
	```
# ---

Tags: #
Topics: [[spread syntax]] [[rest syntax]] [[javascript]]




