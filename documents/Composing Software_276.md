
# Composing Software
*(.toc)*
## [Introduction](https://medium.com/javascript-scene/composing-software-an-introduction-27b72500d6ea)
> Composition: “The act of combining parts or elements to form a whole.” ~ Dictionary.com

Function composition is the process of passing the return value of one function as an argument to another function. In mathematical notation:

`(f ∘ g)(x) = f(g(x))`

Method chaining is the process of directly calling a method on the return value of a function, without needing to refer to the return value by name:

```js
const g = n => n + 1;
const f = n => n * 2;
const wait = time => new Promise(
  (resolve, reject) => setTimeout(
    resolve,
    time
  )
);
wait(300)
  .then(() => 20)
  .then(g)
  .then(f)
  .then(value => console.log(value)) // 42
;
```
> If you’re chaining, you’re composing.
```js
const trace = label => value => {
  console.log(`${ label }: ${ value }`);
  return value;
};

trace('your name:')('jason')
/*
your name: jason
*/
```
```js
// pipe(...fns: [...Function]) => x => y
const pipe = (...fns) => x => fns.reduce((y, f) => f(y), x);
```
> Writing functions without mention of the arguments is called **point-free style**. To do it, you'll call a function that returns the new function, rather than declaring the function explicitly. That means you won't need the `function` keyword or the arrow syntax (`=>`).

## Currying

> **Currying**: A curried function is a function that takes multiple parameters one at a time: It takes a parameter, and returns a function that takes the next parameter, and so on until all parameters have been supplied, at which point, the application is completed and the final value is returned.

```js
// Tiny, recursive autocurry
const curry = (
  f, arr = []
) => (...args) => (
  a => a.length === f.length ?
    f(...a) :
    curry(f, a)
)([...arr, ...args]);
```
```js
const add3 = curry((a, b, c) => a + b + c);

add3(1, 2, 3); // 6
add3(1, 2)(3); // 6
add3(1)(2, 3); // 6
add3(1)(2)(3); // 6
```
## Favor object composition over class inheritance
* **The tight coupling problem**: Because child classes are dependent on the implementation of the parent class, class inheritance is the tightest coupling available in object oriented design.
* **The fragile base class problem**: Due to tight coupling, changes to the base class can potentially break a large number of descendant classes — potentially in code managed by third parties. The author could break code they’re not aware of.
* **The inflexible hierarchy problem**: With single ancestor taxonomies, given enough time and evolution, all class taxonomies are eventually wrong for new use-cases.
* **The duplication by necessity problem**: Due to inflexible hierarchies, new use cases are often implemented by duplication, rather than extension, leading to similar classes which are unexpectedly divergent. Once duplication sets in, it’s not obvious which class new classes should descend from, or why.
* **The gorilla/banana problem**: “…the problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle.” ~ Joe Armstrong, “Coders at Work”
https://medium.com/javascript-scene/a-functional-programmers-introduction-to-javascript-composing-software-d670d14ede30

## Higher Order Functions 

> A higher order function is a function that takes a function as an argument, or returns a function.

### Reduce

```js
const reduce = (reducer, initial, arr) => {
  // shared stuff
  let acc = initial;
  for (let i = 0, { length } = arr; i < length; i++) {
    // unique stuff in reducer() call
    acc = reducer(acc, arr[i]);
  // more shared stuff
  }
  return acc;
};
reduce((acc, curr) => acc + curr, 0, [1,2,3]); // 6
```

```js
const filter = (
  fn, arr
) => reduce((acc, curr) => fn(curr) ?
  acc.concat([curr]) :
  acc, [], arr
);
```

```js
const map = (fn, arr) => arr.reduce((acc, item, index, arr) => {
  return acc.concat(fn(item, index, arr));
}, []);
```

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
```
> Note: pipe is from left to right

### A Word on Redux

```js
reducer(state: Any, action: { type: String, payload: Any}) => newState: Any
```

* A reducer called with no parameters should return its valid initial state.
* If the reducer isn’t going to handle the action type, it still needs to return the state.
* Redux reducers must be pure functions.

redux reducers reduce on actions by time
