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