# ngrx/store
[[TOC]]

[note from Comprehensive introduction to @ngrx/store](https://gist.github.com/btroncone/a6e4347326749f938510)

your store can be thought of as a client side ‘single source of truth’, or database. 

**reducer interface**
```ts
export interface Reducer<T> {
  (state: T, action: Action): T;
}

export const counter: Reducer<number> = (state: number = 0, action: Action) => {
    switch(action.type){
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
};
```

**Action Interface**
```ts
export interface Action {
  type: string;
  payload?: any;
}
//simple action without a payload
dispatch({type: 'DECREMENT'});

//action with an associated payload
dispatch({type: ADD_TODO, payload: {id: 1, message: 'Learn ngrx/store', completed: true}
```
**State Projection**

```ts
//most basic example, get people from state
store.select('people')
  
//combine multiple state slices
Observable.combineLatest(
  store.select('people'),
  store.select('events'),
  (people, events) => {
    //projection here
})
```
**Equipping Store with scan**
```ts

```


**distinctUntilChanged With Store**
```ts
class Dispatcher extends Rx.Subject{
  dispatch(value : any) : void {
    this.next(value);
  }
}

class Store extends Rx.BehaviorSubject{
  constructor(
    private dispatcher,
    private reducer,
    preMiddleware,
    postMiddleware,
    initialState = {}
  ){
    super(initialState);
    this.dispatcher 
       .let(preMiddleware)
       .scan((state, action) => this.reducer(state, action), initialState)
       .let(postMiddleware) 
       .subscribe(state => super.next(state));
  }
  //create basic middleware that logs actions before reducer, and newly outputted state
  //Middleware has been removed since ngrx/store v2.
const preMiddleware = obs => { return obs.do(val => console.log('ACTION: ', val))};
const postMiddleware = obs => { return obs.do(val => console.log('STATE: ', val))};

  /*
    distinctUntilChanged only emits new values when output is distinct, per last emitted value. 
    In the example below, the observable with the distinctUntilChanged operator will emit one less value then the other       with only the map operator applied
  */
  select(key : string) {
    return this
      .map(state => state[key])
      .distinctUntilChanged();
  }

  //...store implementation
}
// add reducers...
// configure store...

const subscriber = store
  //with distinctUntilChanged
  .select('person')
  .subscribe(val => console.log('PERSON WITH DISTINCTUNTILCHANGED:', val));
const subscriberTwo = store
  //without distinctUntilChanged, will print out extra time
  .map(state => state.person)
  .subscribe(val => console.log('PERSON WITHOUT DISTINCTUNTILCHANGED:', val));
//dispatch a few actions
dispatcher.dispatch({
  type: 'ADD_INFO', 
  payload: {
    name: 'Brian', 
    message: 'Exploring Reduce!'
  }
});
//person will not be changed
dispatcher.dispatch({
  type: 'ADD_HOUR'
});
```