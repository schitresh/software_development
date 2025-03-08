## Arrays
- Mutable: Any item can be modified, removed or added
- The elements can be of any data type (unlike C, C++, Java)

```rb
subjects = ['Physics', 'Chemistry', 'Maths']
items = [25.50, True, -50, 1+2j, 'Physics']
Array(1) # [1]
Array([1]) # [1]

[1, 2] + [3, 4] # [1, 2, 3, 4]
[1] * 4 # [1, 1, 1, 1]
[{}] * 4 # [{}, {}, {}, {}]
Array.new(4, 1) # [1, 1, 1, 1]
Array.new(4, {}) # [{}, {}, {}, {}]
{ one: 1, two: 2 }.to_a # [[:one, 1], [:two, 2]]

# For 2D array, don't define like [[1] * 4] * 2 or Array.new(2, Array.new(4, 1))
# Because it will update every change in each row
# For example, num[0][2] = 2 will result in [[1, 1, 2, 1], [1, 1, 2, 1]]
Array.new(2) { [1] * 4 }
Array.new(2) { Array.new(4, 1) }
```

### Accessing
```rb
subjects[1] # 'Chemistry'
subjects[1] = 'Statistics'

items.values_at(1, 3) # Elements at index 1 & 3
items.first # First element
items.first(3) # First 3 elements
items.last
items.last(3)
items.take # Chooses an element randomly
items.take(3) # Chooses 3 elements randomly

[1, 2, 3].include?(2) # True
```

### Unpacking
```rb
x, y = [1, 2] # x = 1, y = 2
x, *y = [1, 2, 3] # x = 1, y = [2, 3]
x, *y, z = [1, 2, 3, 4] # x = 1, y = [2, 3], z = 4
x, *y, z = [1, 4] # x = 1, y = [], z = 4
```

## Slicing
```rb
# From left, the indexes are 0, 1, 2, ...
# From right, the indexes are ..., -3, -2, -1

num = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
num[..4] # [1, 2, 3, 4, 5]
num[...5] # [1, 2, 3, 4, 5]
num[6..] # [7, 8, 9, 10]
num[..-7] # [1, 2, 3, 4]

num[2..4] # [3, 4, 5]
num[-5..-3] # [6, 7, 8]
num[6..-3] # [7, 8]

num[6..][..2] # [7, 8, 9]

num[2..3] = [6, 7] # [1, 2, 6, 7, 5]
num[2..3] = [6, 7, 8, 9] # Adds extra items as well: [1, 2, 6, 7, 8, 9, 5]
num[2..3] = [6] # Removes existing items: [1, 2, 6, 5]
```

## Instance Methods
### General
```rb
length
empty? # nil?
# present? & blank? are rails methods
max
min
sum # Nesting not allowed: [[1, 2], [3]]
sort # sort!
reverse # reverse!
flatten # Flattens array from [[[1, 2]], [3, 4]] to [1, 2, 3, 4]
# Refer: iterators
```

### Insert
```rb
<< obj # Add obj using shovel operator
push(obj) # Add obj at end
unshift(obj) # Add obj at beginning
insert(index, obj)
dup # Duplicate, doing list2 = list1 will only refer list2 to list1
```

### Remove
```rb
pop # Removes last item
pop(num) # Removes last num items
shift # Removes first item
shift(num) # Removes first num items
delete_at(index)
delete(element)

compact # Removes nil elements
uniq # Removes duplicates
clear # Remove all the items
```

### Find
```rb
count(obj) # Count of occurences of obj
index(obj) # Lowest index where obj occurs
```
