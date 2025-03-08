## Arrays
- Container that can hold a fix number of items of the same type
- Can be defined for integer, float and unicode characters

```py
import array
# array.array(typecode, initializer = None)
items = array.array('i', [1, 2, 3]) # Integer
items = array.array('d', [1.5, 2.2]) # Float
items = array.array('u', 'hello') # Unicode character
items = array.fromlist([1, 2, 3])

```
### Accessing
```py
items[1]
items[1] = 4
```

## Slicing
- Same as list

## General Methods
```py
import copy
copy.deepcopy(items)
sorted(items)
```

## Instance Methods
```py
append(element)
insert(index, element)
extend(array2) # Adds all elements from another array
tolist()
fromlist(list1) # Adds all elements from a list

pop()
pop(index)
remove(element)

index(element)
count(element)

reverse()
```
