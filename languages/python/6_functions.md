## Function
```py
def is_even(number):
  return number % 2 == 0

is_even(4)
```

## Pass by Reference or Value
- Behavior of arguments depends on whether they are mutable or immutable

### Immutable Arguments
- If the passed object is immutable (like numeric), arguments are pass by value
- Changing their value creates a new object in memory and leaves the original variable unchanged

```py
def process(value):
  value = value + 1
  print(value) # 11

value = 10
process(value)
print(value) # 10
```

### Mutable Arguments
- If the passed object is mutable, arguments are pass by reference
- Since python variable is a reference to the object in memory

```py
def process(array):
  array.append(3)
  print(array) # [1, 2, 3]

array = [1, 2]
proces(array)
print(array) # [1, 2, 3]
```

## Arguments
```py
# Positional arguments
# Required by the function in correct positional order
def process(val1, val2):
  print(val1, val2)

# Keyword arguments
# When arguments are identified by name in functional call
# Should be used after positional arguments
def process(val1):
  print(val)
process(val1 = 'hello')

# Default arguments
# Default value is used if no value is passed
def process(val1 = 5):
  print(val1)

# Arbitrary arguments
# Variable number of arguments
# Should be used after positional arguments
def add(*args):
  return sum(args)

add(10, 20, 30, 40)

def process(*args, **kwargs):
  sum(args)
  print(kwargs)

process(10, 20, name = 'John Wick', address = 'New York')
```

## Annotations
- Used to add additional metadata about arguments and return data type
- Since python is dynamically typed language
  - It doesn't enforce any type checking at runtime
  - No error is raised if the data type is different
- They are ignored at runtime

```py
# With data type
def add(a: int, b: int):
  return a + b

# With return type
def add(a: int, b: int) -> int:
  return a + b

# With expression
def total(a: 'Marks in Physics', b: 'Marks in Chemistry', c: 'Marks in Maths') -> int:
  return a + b + c

# With default arguments
def percent(a: 'physics', b: 'maths', c: 'max_marks' = 100) -> int:
  return ((a + b) * 100)/(2 * c)

# With arbitrary arguments
def process(*args: 'numbers', **kwargs: 'data'):
  pass

# Multiple information
def division(
  num: dict(type=float, msg: 'numerator'),
  den: dict(type=float, msg: 'denominator')
) -> float:
  return num/den
```
