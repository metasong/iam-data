# cs2310: python numpy
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
```