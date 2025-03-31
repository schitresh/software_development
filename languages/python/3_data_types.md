## Data Types
- Define the type of data stored in variables
- `None` data type is used to denote nothing

## Boolean
```py
# bool
flag = True
in_process = False
```

## Numeric
```py
# Immutable

# int
a = 10

# float
b = 10.25

# complex
c = 10 + 2j
```

## String
```py
# str
# Immutable
text = 'Hello World!'
text = "Hello World!"
para = '''Multiline
String'''
```

## Sequence
- Bounded and iterable
- Can store elements with different data types (as opposed to C)

```py
# list
# Mutable (elements and size can be changed)
items = [1, 2.5, 'one', 'two', [3, 4]]
items[2] # 'one'

# tuple
# Immutable (cannot be updated)
# Read-only lists
items = (1, 2.5, 'one', 'two', [3, 4])
items[2] # 'one'

# range
# Immutable sequence of numbers (used to iterate)
# range(start, stop, step)
range(3) # 0, 1, 2
range(5, 8) # 5, 6, 7
range(5, 15, 3) # 5, 8, 11
range(8, 5, -1) # 8, 7, 6
for i in range(3): print(i) # Will print 0, 1, 2
```

## Mapping
```py
# dict
# Keys are usually strings or numbers but can be almost any type
number_map = { 1: 'one', 2: 'two' }
number_map[1] # 'one'

word_map = { 'one': 1, 'two': 2 }
word_map['one'] # 1
```

## Set
```py
# set
# Stores unique immutable objects (int, float, complex, bool, string, tuple)
# But it itself is mutable
# Not indexed or ordered:
# The position of items is optimized by python
# To perform operations over set as defined in mathematics
items = {1, 2.5, 'one', 'two'}

# frozenset
# Imutable set
```

## Binary
- Commonly used while dealing with files, images, network packets, etc

```py
# bytes
# Each byte is an integer value between 0 & 255
b = b'Hello'
b = bytearray([72, 101, 108, 108, 111]) # b'Hello'

# bytearray
# mutable
a = b'Hello'
b = bytearray([72, 101, 108, 108, 111])
b = bytearray('Hello', 'utf-8')

# memoryview
# Provides a view into the memory of the original object
# Generally objects that support the buffer protocol like bytes & bytearray
data = bytearray(b'Hello')
view = memoryview(data)
print(view) # <memory at 0x00000186FFAA3580>
```

## Type Casting
### Implicit Casting
- When a language compiler/interpreter automatically converts an object
- Python is strongly typed
  - Doesn't allow automatic type conversion between unrelated data types
  - Examples
    - 1 + '2' will raise an error
    - 1 + 2.5 = 3.5
    - 1 + True = 2
- Object with lesser byte size is upgraded to match the larger byte size
  - In 1 + 2.5, 1 will be upgraded to float as 1.0
  - int occupies 4 bytes, float occupies 8 bytes
  - This is because converting 2.5 to int will result in loss of data

### Explicit Casting
- Using python's built-in functions to perform explicit conversions
  - Such as string to integer

```py
str(100) # '100'
str(10.5) # '10.5'
str([1, 2]) # '[1, 2]'

int('100') # 100
int('110', 2) # Binary: 6
int('2A9', 16) # Hex: 681
int(10.5) # 10
int('Hello') # Raises error
float(100) # 100.0

list('Hello') # ['H', 'e', 'l', 'l', 'o']
list((1, 2)) # [1, 2]
tuple([1, 2]) # (1, 2)
list(range(2, 6)) # [2, 3, 4, 5]
```
