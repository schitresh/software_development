## Iterators
```py
# seq can be any iterable like list, tuple, set, etc.
# func can be regular function or lambda function

# Returns True or False
all(seq)
any(seq)

enumerate(seq) # Returns iterable with tuples like (index, item)
iter(seq) # Returns iterable
next(it) # To get the next object from iterable like iter or reversed

# Returns reversed iterable
# Casting is required to get a list: list(reversed(seq))
reversed(seq)

# Returns a list with items sorted
# key is used for custom sorting function
# rev can be true or false
sorted(seq, key = func, reverse = rev)

# Zips sequences in form of tuple and returns an iterable
# list(zip(['one', 'two'], [1, 2], [3, 4])) will return [('one', 1, 3), ('two', 2, 4)]
zip(seq1, seq2, ...)

# Returns iterable
# list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4])) will return [2, 4]
filter(func, seq)
map(func, seq)
```
