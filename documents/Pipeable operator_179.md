# Pipeable operator
[note](https://github.com/ReactiveX/rxjs/blob/master/doc/pipeable-operators.md)
```ts
import { interval } from 'rxjs/observable/interval';
import { filter, map, take, toArray } from 'rxjs/operators';

const takeEveryNth = (n: number) => <T>(source: Observable<T>) =>
  new Observable<T>(observer => {/*(obsver):subscription subscribe function*/
    let count = 0;
    return source.subscribe({
      next(x) {
        if (count++ % n === 0) observer.next(x);
      },
      error(err) { observer.error(err); },
      complete() { observer.complete(); }
    })
  });

/**
 * You can also use an existing operator like so
 */
const takeEveryNthSimple = (n: number) => <T>(source: Observable<T>) =>
  source.pipe(filter((value, index) => index % n === 0 ))

/**
 * And since pipeable operators return functions, you can further simplify like so
 */
const takeEveryNthSimplest = (n: number) => filter((value, index) => index % n === 0);

interval(1000)/*ex.0->O*/.pipe(/*ex.2->O*/
  takeEveryNth(2),/*ex.1->(O=>O):FO*/ /*FO ex.4->O auto conneced with last O*/ 
  map(x => x + x),
  takeEveryNthSimple(3),
  map(x => x * x),
  takeEveryNthSimplest(4),
  take(3),
  toArray()
)
.subscribe/*ex.3->subscription*/(x => console.log(x));
// [0, 2304, 9216]
```
* `P`ipeable operator is a `function-P` that take configuraton parameters and return another `function-O` which take an observable and return another observable
