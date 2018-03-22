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
**Associating the Dispatcher with Store**
```ts
/*
All actions should pass through pipeline before newly calculated state is passed to store. 
1.) Dispatched Action
2.) Pre-Middleware
3.) Reducers (return new state)
4.) Post-Middleware
5.) store.next(newState)
*/

class Dispatcher extends Rx.Subject{
  dispatch(value : any) : void {
    this.next(value);
  }
}

class Store extends Rx.BehaviorSubject{
  constructor(
    private dispatcher,
    initialState
  ){
    super(initialState);
    /* 
    all dispatched actions pass through action pipeline before new state is passed
    to store
    */
    this.dispatcher 
       //pre-middleware
       //reducers
       //post-middleware 
       .subscribe(state => super.next(state)); 

  }
  //delegate store.dispatch first through dispatched action pipeline
  dispatch(value){
    this.dispatcher.dispatch(value);  
  }
  //override store next to allow direct subscription to action streams by store
  next(value){
    this.dispatcher.dispatch(value);
  }
}

const dispatcher = new Dispatcher();
const store = new Store(dispatcher, 'INITIAL STATE');

const subscriber = store.subscribe(val => console.log(`VALUE FROM STORE: ${val}`));
/*
All dispatched actions first flow through action pipeline, calculating new state which is
then passed to store. To recap, our ideal behavior:
dispatched action -> pre-middleware -> reducers -> post-middleware -> store.next(newState)
*/

//both methods are same behind the scenes
dispatcher.dispatch('DISPATCHED VALUE!');
store.dispatch('ANOTHER DISPATCHED VALUE!');

const actionStream$ = new Rx.Subject();
/*
Overriding store next method allows us to subscribe store directly to action streams, providing same behavior as manually calling store.dispatch or dispatcher.dispatch
*/
actionStream$.subscribe(store);
actionStream$.next('NEW ACTION!');
```
**Store / Redux Style Reducer**
```ts
//Redux-Style Reducer
const person = (state = {}, action) => {
  switch(action.type){
    case 'ADD_INFO':
      return Object.assign({}, state, action.payload)
    default:
      return state;
  }
}

const infoAction = {type: 'ADD_INFO', payload: {name: 'Brian', framework: 'Angular'}}
const anotherPersonInfo = person(undefined, infoAction);
console.log('***REDUX STYLE PERSON***: ', anotherPersonInfo);

//Add another reducer
const hoursWorked = (state = 0, action) => {
  switch(action.type){
    case 'ADD_HOUR':
      return state + 1;
    case 'SUBTRACT_HOUR':
      return state - 1;
    default:
      return state;
  }
}
//Combine Reducers Refresher
const myReducers = {person, hoursWorked};
const combineReducers = reducers => (state = {}, action) => {
  return Object.keys(reducers).reduce((nextState, key) => {
    nextState[key] = reducers[key](state[key], action);
    return nextState;
  }, {});
};
/*
This gets us most of the way there, but really want we want is for the value of firstState and secondState to accumulate
as actions are dispatched over time. Luckily, RxJS offers the perfect operator for this scenario., to be discussed in next lesson.
*/
const rootReducer = combineReducers(myReducers);
const firstState = rootReducer(undefined, {type: 'ADD_INFO', payload: {name: 'Brian'}});
const secondState = rootReducer({hoursWorked: 10, person: {name: 'Joe'}}, {type: 'ADD_HOUR'});
console.log('***FIRST STATE***:', firstState);
console.log('***SECOND STATE***:', secondState);
```

**Equipping Store with scan**
```ts
class Store extends Rx.BehaviorSubject{
  constructor(
    private dispatcher,
    private reducer,
    initialState = {}
  ){
    super(initialState);
    this.dispatcher 
       //pre-middleware?
/*
Scan is a reduce over time. In the previous lesson we compared reduce to a snowball rolling downhill, accumulating mass
(or calculated value). Scan can be thought of similarly, except the hill has no certain end. The accumulator (in this
case, state) will continue to collect until destroyed. This makes it the ideal operator for managing application state.
*/
       .scan((state, action) => this.reducer(state, action), initialState)
       //post-middleware? 
       .subscribe(state => super.next(state));
  }
  //...store implementation
}
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