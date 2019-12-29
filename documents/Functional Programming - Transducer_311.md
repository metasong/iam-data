# Functional Programming - Transducer
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---
> Transduce: Derived from the 17th century scientific latin, “transductionem” means “to change over, convert”. It is further derived from “transducere/traducere”, which means “to lead along or across, transfer”.

* Composable using simple function composition
* Efficient for large collections or infinite streams: Only enumerates over the elements once, regardless of the number of operations in the pipeline
* Able to transduce over any enumerable source (e.g., arrays, trees, streams, graphs, etc…)
* Usable for either lazy or eager evaluation with no changes to the transducer pipeline; Transducers don’t care whether you pull or push. Transducers have no awareness of the data structure they’re acting on. They simply call the reducer you pass into them to accumulate new values.




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
## Example from Ramda

```js
import {
  compose,
  filter,
  map,
  into
} from 'ramda';
const isEven = n => n % 2 === 0;
const double = n => n * 2;
const doubleEvens = compose(
  filter(isEven),
  map(double)
);
const arr = [1, 2, 3, 4, 5, 6];
// into = (structure, transducer, data) => result
// into transduces the data using the supplied
// transducer into the structure passed as the
// first argument.
const result = into([], doubleEvens, arr);
console.log(result); // [4, 8, 12]
```

https://medium.com/javascript-scene/transducers-efficient-data-processing-pipelines-in-javascript-7985330fe73d


