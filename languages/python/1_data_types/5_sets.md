## Sets
- Stores unique objects
- Mutable, but can store only immutable data types
- Unordered collection
  - Items are not accessible by positional index
  - Order of items can be changed for optimization
  - This is useful for set operations

```py
subjects = {'Physics', 'Chemistry', 'Maths'}
items = {25.50, True, -50, 1+2j, 'Physics'}
items = {1, 2, 2, 2, 3} # {1, 2, 3}

# Assigning mutable data types will raise error
# items = {[1, 2]}

# Any type of conatenation will raise error
# {1, 2} + {3, 4}
# {1} * 4
{x * 2 for x in range(5, 10)} # [10, 12, 14, 16, 18]
{x * 2 for x in range(5, 10) if x % 2 == 0} # [12, 16]

# Joining & Intersection
{1, 2} | {3, 4} # {1, 2, 3, 4}
{1, 2} & {2, 4} # {2}
{1, 2}.union({3, 4}) # {1, 2, 3, 4}
{*s1, *s2} # {1, 2, 3, 4} if s1 is {1, 2} & s2 is {3, 4}

# 2D sets cannot be created since set is mutable
# {{1} * 4 for _ in range(2)} will raise error
```

### Accessing
```py
# Can only traverse through a set
# Using positional index will raise error
# subjects[1]
# subjects[1] = 'Statistics'
# del subjects[1]

2 in {1, 2, 3} # True
2 not in {1, 2, 3} # False
```

### Unpacking
```py
x, y = {1, 2} # x = 1, y = 2
x, *y = {1, 2, 3} # x = 1, y = [2, 3]
x, *y, z = {1, 2, 3, 4} # x = 1, y = [2, 3], z = 4
x, *y, z = {1, 4} # x = 1, y = [], z = 4
```

## Slicing
- Not supported since positional indexes are not supported

## General Methods
```py
len(items)
max(items)
min(items)
sum(items)
list(seq) # Converts sequence or iterator to list
sorted(items) # Converts to list
# reversed(items) not supported
# Refer: iterators
```

## Instance Methods
```py
# Insert
add(obj)
update(seq) # Add the contents of any seq to the set
union(seq) # Similar to union but returns a new set object
copy() # Doing set2 = set1 will only refer set2 to set1

# Remove
pop() # Removes the first item from set
remove(obj) # Removes object and raises error if not present
discard(obj) # Removes object but doesn't raise error if not present
clear() # Remove all the items

# Find
count(obj) # Count of occurences of obj
index(obj) # Lowest index where obj occurs
```

### Set functions
```py
# New set after removing items common with obj
# Same as a - b
difference(obj)
# New set after removing items uncommon with obj
# Same as a & b
intersection(obj)
# New set after adding uncommon items and removing common items
# Same as a ^ b
symmetric_difference(obj)

difference_update(obj) # Removes items common with obj
intersection_update(obj) # Removes items uncommon with obj
symmetric_difference_update(obj) # Adds uncommon items and removes common items

isdisjoint(seq) # True if the two sequences have no common element
issubset(seq) # True if set is subset of seq
issuperset(seq) # True if set is superset of seq
```
