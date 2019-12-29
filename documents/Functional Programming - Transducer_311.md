# Functional Programming - Transducer
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---
```js
const compose = (...fns) => x => fns.reduceRight((y, f) => f(y), x);
const map = f => step =>
  (a, c) => step(a, f(c));
const filter = predicate => step =>
  (a, c) => predicate(c) ? step(a, c) : a;
const isEven = n => n % 2 === 0;
const double = n => n * 2;
const doubleEvens = compose(
  filter(isEven),
  map(double)
);
const arrayConcat = (a, c) => a.concat([c]);
const xform = doubleEvens(arrayConcat);
const result = [1,2,3,4,5,6].reduce(xform, []); // [4, 8, 12]
console.log(result);
```

## Transducer Rules

1. Initialization: Given no initial accumulator value, a transducer must call the step function to produce a valid initial value to act on. The value should represent the empty state. For example, an accumulator that accumulates an array should supply an empty array when its step function is called with no arguments.
1. Early termination: A process that uses transducers must check for and stop when it receives a reduced accumulator value. Additionally, a transducer step function that uses a nested reduce must check for and convey reduced values when they are encountered.
1. Completion (optional): Some transducing processes never complete, but those that do should call the completion function to produce a final value and/or flush state, and stateful transducers should supply a completion operation that cleans up any accumulated resources and potentially produces one final value.

```js
// import a standard curry, or use this magic spell:
const curry = (
  f, arr = []
) => (...args) => (
  a => a.length === f.length ?
    f(...a) :
    curry(f, a)
)([...arr, ...args]);
const transduce = curry((step, initial, xform, foldable) =>
  foldable.reduce(xform(step), initial)
);

```


