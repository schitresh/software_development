## Dictionaries
- Keys should be immutable like number, string or tuple

```py
subjects = { 'Phy': 'Physics', 'Chem': 'Chemistry', 'Math': 'Mathematics' }
numbers = { 1: 'one', 2: 'two' }
category = { (1, 'a'): 'A', (2, 'b'): 'B' }
empty_dict = {}

dict.from_keys(['a', 'b']) # { 'a': None, 'b': None }
dict.from_keys(['a', 'b'], 100) # { 'a': 100, 'b': 100 }

values = [1]
dict.from_keys(['a', 'b'], values) # { 'a': [1], 'b': [1] }
# Dict will refer to values variable, any change to it will be reflected
values.append(2) { 'a': [1, 2], 'b': [1, 2] }

dict([('a', 100), ('b', 200)]) # From list of tuples: { 'a': 100, 'b': 200 }
dict([['a', 100], ['b', 200]]) # From list of lists: { 'a': 100, 'b': 200 }
dict(a = 100, b = 200) # From keywords: {'a': 100, 'b': 200}

{ 'a': 1, 'b': 2 } | { 'c': 3 } # { 'a': 1, 'b': 2, 'c': 3 }
{ **dict1, **dict2 }
```

### Accessing
```py
# If key is not present, it will raise error
# subjects['ab'] will raise key error
subjects['Phy'] # 'Physics', using 'phy' as key string is case sensitive

# Doesn't raise error if key is not present
subjects.get('Math') # Mathematics

subjects['Math'] = 'Algebra'
subjects['Stat'] = 'Statistics'
del subjects['Math']

for code in subjects: print(code)
for code, name in subjects.items(): print(code, name)
```

## General Methods
```py
len(items)
str(items)
```

## Instance Methods
```py
# Return view objects that are refreshed dynamically whenever anything changes
# Need to typecast to get a list from them
keys()
values()
items() # View object of key-value tuples

has_key(key)
# Returns default if key not present in dict, doesn't raise error
get(key, default = None)
# Similar to get, but adds the key with default value if not present
setdefault(key, default = None)

# Updates the existing keys from dict2 and adds new keys
# List of lists or tuples, or keywords can also be used instead of dict2
update(dict2)
copy()

pop(key) # Removes the key and returns the value
popitem() # Removes the last inserted key and returns the key-value tuple
clear()
```
