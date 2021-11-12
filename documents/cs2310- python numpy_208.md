# cs2310- python numpy
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
subPage: []
enable: [toc]

---
https://cs231n.github.io/python-numpy-tutorial/

Python does not have unary increment (x++) or decrement (x--) operators.

Booleans: Python implements all of the usual operators for Boolean logic, but uses English words rather than symbols (&&, ||, etc.)

xor is: !=
```python
print(t != f)  # Logical XOR; prints "True" 
```
```python
x=2
print(x,type(x),x**3) # 2 <class 'int'> 8
hw12 = '%s %s %d' % (hello, world, 12)  # sprintf style string formatting
print(hw12)  # prints "hello world 12"
```
## Containers
### list
```python
x= [3,'cat',1,2]
print(x[1:3]) # ['cat',1] :get [1,3)
print(x[-1] # 2 :index is lenth-1

# slice is just a map of original list
nums[2:4] = [8, 9]        # Assign a new sublist to a slice
print(nums)               # Prints "[0, 1, 8, 9, 4]"

```

