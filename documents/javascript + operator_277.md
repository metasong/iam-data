# javascript + operator
There are two kinds of values in JavaScript: primitives and objects. The primitive values are: undefined, null, booleans, numbers, and strings. All other values are objects, including arrays and functions.

The plus operator performs three kinds of conversion: It converts values to primitives, numbers and strings:

## 'ToPrimitive(input, PreferredType?)'
The optional parameter PreferredType is either 'Number or String. It only expresses a preference, the result can always be any primitive value. If PreferredType is Number, the following steps are performed to convert a value input (§9.1):
1. If input is primitive, return it as is. 
1. Otherwise, input is an object. Call obj.valueOf(). If the result is primitive, return it.
1. Otherwise, call obj.toString(). If the result is a primitive, return it.
1. Otherwise, throw a TypeError.
If PreferredType is String, steps 2 and 3 are swapped. If PreferredType is missing then it is set to String for instances of Date and to Number for all other values.


## References
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators
http://2ality.com/2012/01/object-plus-object.html

