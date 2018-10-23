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
[Symbol.toPrimitive](hint)
// The hint argument can be one of "number", "string", and "default".
```

If hint is Number:
1. If input is primitive, return it as is.
1. Otherwise, input is an object. Call obj.[valueOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf). If the result is primitive, return it.
1. Otherwise, call obj.toString(). If the result is a primitive, return it.
1. Otherwise, throw a TypeError.
>  
> for Date: If hint is "string" or "default", [@@toPrimitive]() tries to call the toString method. If the toString property does not exist, it tries to call the valueOf method and if the valueOf does not exist either, [@@toPrimitive]() throws a TypeError. If hint is "number", [@@toPrimitive]() first tries to call valueOf, and if that fails, it calls toString.

> the valueOf() method returns the primitive value of the specified object.

If PreferredType is String, steps 2 and 3 are swapped. If PreferredType is missing then it is set to String for instances of Date and to Number for all other values.
```js
// An object without Symbol.toPrimitive property.
var obj1 = {};
console.log(+obj1);     // NaN
console.log(`${obj1}`); // "[object Object]"
console.log(obj1 + ''); // "[object Object]"

// An object with Symbol.toPrimitive property.
var obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint == 'number') {
      return 10;
    }
    if (hint == 'string') {
      return 'hello';
    }
    return true;
  }
};
console.log(+obj2);     // 10        -- hint is "number"
console.log(`${obj2}`); // "hello"   -- hint is "string"
console.log(obj2 + ''); // "true"    -- hint is "default"
```
### toNumber()
Argument|	Result
-|-
undefined|	NaN
null|	+0
boolean value|	true is converted to 1, false is converted to +0
number value|	no conversion necessary
string value|	parse the number in the string. For example, "324" is converted to 324, '' is converted to 0.

### toString()

Argument|	Result
-|-
undefined|	"undefined"
null|	"null"
boolean value|	either "true" or "false"
number value|	the number as a string, e.g. "1.765"
string value	|no conversion necessary

## test
```js
[]+[]
// ""
// object.__proto__.valueOf return this.
// Array.__proto__.toString return ''

[]+{}
// '[object Object]'
6 + { valueOf: function () { return 2 } }
// 8
"abc" + { toString: function () { return "def" } }
// 'abcdef'
{}+{}
// NaN
// the first {} is parsed as statement. + conbert the next {} to number
({}+{})
// [object Object][object Object]
// () create an expression
{}+[]
// 0
// +[] => Number([].toString()) => Number('') => 0;
```


