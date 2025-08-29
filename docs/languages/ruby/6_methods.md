## Method
- Last statement is automatically returned

```rb
def is_even(number)
  number % 2 == 0
end
is_even(4)

def is_even(number)
  return unless number.is_a?(Integer)
  number % 2 == 0
end
is_even(4)

alias is_even_alias is_even
```

## Pass by Reference or Value
- Behavior of arguments depends on whether they are mutable or immutable

### Immutable Arguments
- If the passed object is immutable (like numeric), arguments are pass by value
- Changing their value creates a new object in memory and leaves the original variable unchanged

```rb
def process(value)
  value = value + 1
  p(value) # 11
end

value = 10
process(value)
p(value) # 10
```

### Mutable Arguments
- If the passed object is mutable, arguments are pass by reference
- Since variable is a reference to the object in memory

```rb
def process(array)
  array.append(3)
  p(array) # [1, 2, 3]
end

array = [1, 2]
process(array)
p(array) # [1, 2, 3]
```

## Arguments
```rb
# Positional arguments
# Required by the function in correct positional order
def process(val1, val2)
  p(val1, val2)
end

# Keyword arguments
# When arguments are identified by name
# Should be used after positional arguments
def process(val1:)
  p(val)
end
process(val1: 'hello')

# Default arguments
# Default value is used if no value is passed
def process(val1 = 5, val2: 'hello')
  p(val1)
end
process()

# Arbitrary arguments
# Variable number of arguments
# Should be used after positional arguments
def add(*args)
  args.sum
end
add(10, 20, 30, 40)

def process(*args, **kwargs)
  args.sum
  p(kwargs)
end
process(10, 20, name: 'John Wick', address: 'New York')
```
