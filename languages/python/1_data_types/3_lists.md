## Lists
- Mutable: Any item can be modified, removed or added
- The elements can be of any data type (unlike C, C++, Java)

```py
subjects = ['Physics', 'Chemistry', 'Maths']
items = [25.50, True, -50, 1+2j, 'Physics']

[1, 2] + [3, 4] # [1, 2, 3, 4]
[1] * 4 # [1, 1, 1, 1]
[x * 2 for x in range(5, 10)] # [10, 12, 14, 16, 18]
[x * 2 for x in range(5, 10) if x % 2 == 0] # [12, 16]

# For 2D array, don't define like [[1] * 4] * 2
# Because it will update every change in each row
# For example, num[0][2] = 2 will result in [[1, 1, 2, 1], [1, 1, 2, 1]]
[[1] * 4 for _ in range(2)] # [[1, 1, 1, 1], [1, 1, 1, 1]]
```

### Accessing
```py
subjects[1] # 'Chemistry'
subjects[1] = 'Statistics'
del subjects[1] # ['Physics', 'Maths']
2 in [1, 2, 3] # True
2 not in [1, 2, 3] # False
```

### Unpacking
```py
x, y = [1, 2] # x = 1, y = 2
x, *y = [1, 2, 3] # x = 1, y = [2, 3]
x, *y, z = [1, 2, 3, 4] # x = 1, y = [2, 3], z = 4
x, *y, z = [1, 4] # x = 1, y = [], z = 4
```

## Slicing
```py
# From left, the indexes are 0, 1, 2, ...
# From right, the indexes are ..., -3, -2, -1
# Slicing: [start:stop:step]
# With respect to index: [first_index : last_index + 1 : step]

num = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
num[:5] # [1, 2, 3, 4, 5]
num[6:] # [7, 8, 9, 10]
num[:-6] # [1, 2, 3, 4]

num[2:5] # [3, 4, 5]
num[-5:-2] # [6, 7, 8]
num[6:-2] # [7, 8]

num[1:10:3] # [2, 5, 8]
num[6:][:3] # [7, 8, 9]

num[:] # Just clones the string: 'hello world'
num[2:4] = [6, 7] # [1, 2, 6, 7, 5]
num[2:4] = [6, 7, 8, 9] # Adds extra items as well: [1, 2, 6, 7, 8, 9, 5]
num[2:4] = [6] # Removes existing items: [1, 2, 6, 5]
del num[2:4] # [1, 2, 5]
```

## General Methods
```py
len(items)
max(items)
min(items)
sum(items) # Nesting not allowed: sum((1, 2), (3))
list(seq) # Converts sequence or iterator to list
sorted(items)
reversed(items)
# Refer: iterators
```

## Instance Methods
```py
# Insert
append(obj)
extend(seq) # Append the contents of seq to list
insert(index, obj)
copy() # Doing list2 = list1 will only refer list2 to list1

# Remove
pop() # Removes last item
pop(index) # Removes item at index
remove(obj) # Removes first occurence of object
clear() # Remove all the items

# Find
count(obj) # Count of occurences of obj
index(obj) # Lowest index where obj occurs

reverse()
sort(key = func, reverse = rev)
```
