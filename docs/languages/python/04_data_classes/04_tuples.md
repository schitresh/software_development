## Tuples
- Immutable: Items can be accessed but not modified, removed or added
- The elements can be of any data type (unlike C, C++, Java)

```py
subjects = ('Physics', 'Chemistry', 'Maths')
items = (25.50, True, -50, 1+2j, 'Physics')
items = (1,) # To create a single value tuple, comma is required
items = 1, 2, 3 # Any set of comma separated objects defaults to tuple

(1, 2) + (3, 4) # [1, 2, 3, 4]
(1,) * 4 # [1, 1, 1, 1]
tuple(x * 2 for x in range(5, 10)) # [10, 12, 14, 16, 18]
tuple(x * 2 for x in range(5, 10) if x % 2 == 0) # [12, 16]

# For 2D array, don't define like [[1] * 4] * 2
# Because it will update every change in each row
# For example, num[0][2] = 2 will result in [[1, 1, 2, 1], [1, 1, 2, 1]]
tuple((1,) * 4 for _ in range(2)) # [[1, 1, 1, 1], [1, 1, 1, 1]]
```

### Accessing
```py
subjects[1] # 'Chemistry'
# Editing will raise error: subjects[1] = 'Statistics'
# If such operations are required, tuple needs to be converted to list
del subjects
2 in (1, 2, 3) # True
2 not in (1, 2, 3) # False
```

### Unpacking
```py
x, y = (1, 2) # x = 1, y = 2
x, *y = (1, 2, 3) # Multiple items unpacked to list: x = 1, y = [2, 3]
x, *y, z = (1, 2, 3, 4) # x = 1, y = [2, 3], z = 4
x, *y, z = (1, 4) # x = 1, y = [], z = 4
```

## Slicing
- Similar to lists

## General Methods
```py
len(items)
max(items)
min(items)
sum(items) # Nesting allowed: sum((1, 2), (3))
tuple(seq) # Converts sequence or iterator to list
sorted(items) # Converts to list
reversed(items) # Returns iterator
# Refer: iterators
```

## Instance Methods
```py
# Find
count(obj) # Count of occurences of obj
index(obj) # Lowest index where obj occurs
```
