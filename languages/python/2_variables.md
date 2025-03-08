## Variables
- Reserved memory locations used to store values
- When a variable is created, it reserves some space in the memory
  - A stores an object in the memory only once
  - The variable refers to this object and not the memory location
  - If two variables are equated, they refer to the same object
  ```py
  a = b = 10
  a is b # True
  id(a) == id(b) # True
  ```
- If the value of the variable changes
  - It creates a new object with the new value at some other location
  - The old object & memory location remains unreferred
  - Garbage collector releases the memory occupied by unreferred objects
- In contrast, variables in C are memory referenced and changes are overwrittten

```py
# Declaration
name = 'John Wick'
counter = 100

# Get the data type
type(name) # <class 'str'>
type(counter) # <class 'int'>

# Deleting the reference
del name, counter

# Parallel Assignment
a, b, c = 10, 20, 'John Wick'
a = b = c = 10
```

## Types
- Local
  - Can be enlisted using `locals()`
- Global
  - Determined from the scope
  - Defined the same way as local variables
  - Can be enlisted using `globals()`
  - If modified from inside a function, it will raise an error
    - Use the global keyword to refer it before modifying
    - `global variable_name`
- Constant
  - Not formally defined, but can be indicated uusing all caps
  - For example, PI_VALUE
