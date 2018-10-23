# What is {} + {} in JavaScript
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---

http://2ality.com/2012/01/object-plus-object.html

http://2ality.com/2011/12/fake-operator-overloading.html

The general rule for addition in JavaScript is simple: You can only add numbers and strings, all other values will be converted to either one of those types.

## Addition
```js
value1 + value2
```
1. Convert both operands to primitives:
    > prim1 := ToPrimitive(value1)   
    > prim2 := ToPrimitive(value2)
1. PreferredType is omitted and thus Number for non-dates, String for dates.
1. If either prim1 or prim2 is a string then convert both to strings and return the concatenation of the results.
1. Otherwise, convert both prim1 and prim2 to numbers and return the sum of the results.


## Converting

### Converting values to primitives via [toPrimitive](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)  
```js
ToPrimitive(input, PreferredType?)
```

If PreferredType is Number:
1. If input is primitive, return it as is.
1. Otherwise, input is an object. Call obj.valueOf(). If the result is primitive, return it.
1. Otherwise, call obj.toString(). If the result is a primitive, return it.
1. Otherwise, throw a TypeError.
>  
> for Date: If hint is "string" or "default", [@@toPrimitive]() tries to call the toString method. If the toString property does not exist, it tries to call the valueOf method and if the valueOf does not exist either, [@@toPrimitive]() throws a TypeError. If hint is "number", [@@toPrimitive]() first tries to call valueOf, and if that fails, it calls toString.

If PreferredType is String, steps 2 and 3 are swapped. If PreferredType is missing then it is set to String for instances of Date and to Number for all other values.



