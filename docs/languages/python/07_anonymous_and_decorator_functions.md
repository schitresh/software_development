## Anonymous Function
- Defined using lambda keyword
- Returns a single expression that can be stored in a variable
- Have their own local namespace
  - Cannot access variables other than parameter list
  - Cannot access variables in global namespace

```py
add = lambda a, b: a + b
add(10, 20)
```

## Decorator Functions
```py
def test_decorator(test_function):
  def nested_function(value):
    # Wraps the test_function and extends its behavior
    print('hi', end = ', ')
    test_function(value)

  return nested_function

# Method 1: By using decorator
@test_decorator
def test_function(value):
  print('hello')

test_function() # hi, hello

# Method 2: By reassigning
def test_function():
  print('hello')
# Will need to add the 'value' argument in test_decorator
test_function = test_decorator(test_function, value)

test_function() # hi, hello
```
