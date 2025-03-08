## Strings
- Immutable
  - Cannot be modified in-place once stored in a memory location
  - New characters cannot be appended or inserted
  - Any changes will create a new string object
- One hack can be to convert it into list (which is mutable)
  - Perform required operations and join it back

```py
text = 'hello'
text[1] # 'e' (Single char is also a string)
text + 'world' # 'hello world'
text * 3 # 'hellohellohello'

text = "Hello World!"
para = '''Multiline
String'''

'el' in 'hello' # True
'el' not in 'hello' # False

'hello\nworld' # Adds new line
r'hello\nworld' # Raw string, ignores escaped characters, no new line
```

## Formatting
### Formatting Operator
```py
# %s: converts to string using str() before formatting
# %c: character
'Employee name is %s' % 'John Wick'
'Category code is %4s' % 'BD' # min padding 2 chars: 'Category code is   BD'
'%s is in %s department' % ('John Wick', 'Finance')
# %d: integer
# %f: float
'Scored %d out of 20' % 15
'Scored %2d out of 20' % 6 # min padding 2 chars using space: 'Scored  6'
'Scored %02d out of 20' % 6 # min padding 2 chars using 0: 'Scored 06 ...'
'Scored %.2f out of 20' % 6.1 # min 2 zeroes after decimal: 'Scored 6.10'
'Scored %05.2f out of 20' % 6.1 # min padding 5 chars with 0: 'Scored 06.10'
```

### Format Method
```py
'{} is in {} department'.format('John Wick', 'Finance')
'{name} is in {dept} department'.format(name = 'John Wick', dept = 'Finance')
'{:s} scored {:05.2f} out of 20'.format('John', 6.1)
'Category code is {:.2}'.format('BDEG') # Truncation: 'Category code is BD'
```

### F-strings
```py
name = 'John Wick'; dept = 'Finance'
f'{name} is in {dept} department'

price = 100; quantity = 5
f'Total amount: {price * quantity}' # 'Total amount: 500'

# Self debugging expression using '='
# 'Total amount: price * quantity = 500'
f'Total amount: {price * quantity = }'
```

### Templates
```py
from string import Template
template = Template('$name is in $dept department')
template.substitute(name = 'John Wick', dept = 'Finance')

data = { 'name': 'John Wick', 'dept': 'Finance' }
template.substitute(**data)
```

## Slicing
```py
# From left, the indexes are 0, 1, 2, ...
# From right, the indexes are ..., -3, -2, -1
# Slicing: [start:stop:step]
# With respect to index: [first_index : last_index + 1 : step]

text = 'hello world'
text[:5] # 'hello'
text[6:] # 'world'
text[:-6] # 'hello'

text[2:5] # 'llo'
text[-5:-2] # 'wor'
text[6:-2] # 'wor'

text[1:10:3] # 'eoo'
text[6:][:3] # 'wor'

text[:] # Just clones the string: 'hello world'
# Replacing not supported for strings (though supported by lists)
# text[2:4] = 'kk'
```

## General Methods
```py
len(string)
max(string)
min(string)
```

## Instance Methods
- Applied on string like `string.method_name(*args, **kwargs)`

### Find and replace
```py
find(substr, beg = i, end = j) # Returns index if found else -1
# lfind, rfind
index(substr, beg = i, end = j) # Raises exception if not found
# lindex, rindex
count(substr, beg = i, end = j) # Counts occurence of substring
replace(old, new)

startswith(prefix, beg = i, end = j)
endswith(suffix, beg = i, end = j)
removeprefix(prefix)
removesuffix(suffix)
```

### Split and Join
```py
strip # Removes trailing whitespaces
# lstrip, rstrip
split(delimiter) # Deletes delimiter, splits and returns list of subsrings
partition(sep) # Splits the string in 3 tuples on occurence of separator
# lpartition, rpartition
join(seq) # Joins the sequence using the string
```

### Char Types
```py
isalpha()
isdigit() # If all chars are digits (no decimals or fractions)
isnumeric() # If all chars are any numerical value (including decimals, fractions)
isalnum()
```

### Case
```py
islower()
isupper()
istitle()
lower()
upper()
title() # Titlecase
capitalize() # Capitalizes the first letter of string
```
