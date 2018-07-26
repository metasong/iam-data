  # lisp interpreter

..,
....

https://maryrosecook.com/blog/post/little-lisp-interpreter

1. the fence of the metaseed language is ..,
1. ```lisp
    // map
    (1 true 'abc' a:2 b:true;('ef' b:2))
    // or 
        (1 true 'abc' a:2 b:true;('ef' b:2))

    // equas to
    (0:1 1:true 2:'abc' a:2 b:true children:(0:'ef' b:2))
    
    // embedded map
      (1 a:true ;('ef' b:2; c:3))
    ```	
1. ```js  
    fn(1 true a:'abc'; 'de',2) 
    // equals to
    fn(0:1 1:true a:'abc' children:(0:'de' 1:2))
    ```
   
```lisp
..,
a(1 3 b:2;   
  b( '3c' true  
   
  )  
  c(1 3 true  
  	;'abcd'
  )
)
..,
```

```js
  const a =5;
```

ssd