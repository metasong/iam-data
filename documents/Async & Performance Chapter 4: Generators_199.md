# Async & Performance Chapter 4: Generators

https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch4.md

Callback-based async doesn't fit how our brain plans out steps of a task.
Callbacks aren't trustable or composable because of inversion of control.
Promises uninvert the inversion of control of callbacks, restoring trustability/composability.Now we turn our attention to expressing async flow control in a sequential, synchronous-looking fashion. 

```js
var x = 1;

function *foo() {
	x++;
	yield; // pause!
	console.log( "x:", x );
}

function bar() {
	x++;
}

// construct an iterator `it` to control the generator
var it = foo();

// start `foo()` here!
it.next();
x;						// 2
bar();
x;						// 3
it.next();				// x: 3
```


## Iteration Messaging

```js
function *foo(x) {
	var y = x * (yield);
	return y;
}

var it = foo( 6 );

// start `foo(..)`
it.next();

var res = it.next( 7 );

res.value;		// 42
  
////////
function *foo(x) {
	var y = x * (yield "Hello");	// <-- yield a value!
	return y;
}

var it = foo( 6 );

var res = it.next();	// first `next()`, don't pass anything
res.value;				// "Hello"

res = it.next( 7 );		// pass `7` to waiting `yield`
res.value;				// 42
```

## 