# AI Numpy
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
subPage: []
enable: [toc]

---
https://cs231n.github.io/python-numpy-tutorial/

```python
print(t and f) # Logical AND;
print(t or f)  # Logical OR;
print(not t)   # Logical NOT;
# no xor keyword
print(t != f)  # Logical XOR;
```

```python
nums = list(range(5))    # range is a built-in function that creates a list of integers
print(nums)         # Prints "[0, 1, 2, 3, 4]"
print(nums[2:4])    # Get a slice from index 2 to 4 (exclusive); prints "[2, 3]"
print(nums[2:])     # Get a slice from index 2 to the end; prints "[2, 3, 4]"
print(nums[:2])     # Get a slice from the start to index 2 (exclusive); prints "[0, 1]"
print(nums[:])      # Get a slice of the whole list; prints ["0, 1, 2, 3, 4]"
print(nums[:-1])    # Slice indices can be negative; prints ["0, 1, 2, 3]"
nums[2:4] = [8, 9] # Assign a new sublist to a slice
print(nums)         # Prints "[0, 1, 8, 9, 4]"
```

```python
animals = ['cat', 'dog', 'monkey']
for animal in animals:
    print(animal)
    
for idx, animal in enumerate(animals):
    print('#{}: {}'.format(idx + 1, animal))

print('cat' in d)     # Check if a dictionary has a given key; 

# dictionary
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal, legs in d.items():
    print('A {} has {} legs'.format(animal, legs))
    
# set
animals = {'cat', 'dog', 'fish'}
for idx, animal in enumerate(animals):
    print('#{}: {}'.format(idx + 1, animal))
```
```python
# A numpy array is a grid of values, all of the same type
b = np.array([[1,2,3],[4,5,6]])   # Create a rank 2 array
# indexed by a tuple of nonnegative integers.
print(b[0, 0], b[0, 1], b[1, 0])
c = b[:2, 1:3]
```


