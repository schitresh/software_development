## Inheritance
```py
class Parent:
  def __init__(self, name):
    self.name = name

class Child(Parent):
  def __init__(self, name, dept):
    super().__init__(name)
    self.dept = 'Finance'

# Multiple Inheritance
# Child.mro() returns hierarchical order of classes to resolve methods
# For this class, it will return [<class '__main__.Child1'>, <class '__main__.Parent1'>,
#  <class '__main__.Parent2'>, <class 'object'>]
class Child1(Parent1, Parent2):
  pass
```

## Abstraction
```py
# ABC: Abstract Base Class
from abc import ABC, abstractmethood

# Abstract Class or Interface
# Declaring an object of Shape class will raise an error
class Shape(ABC):
  @abstractmethod
  def draw(self):
    raise Exception('Not implemented')

# Concrete Classes
class Circle(Shape):
  def draw(self):
    pass

class Rectangle(Shape):
  def draw(self):
    pass
```

## Polymorphism
```py
# Method Overriding
# Base overridable methods: __init__, __del__, __repr__, __str__
class Parent:
  def display(self):
    print('Parent')

class Child(Parent):
  def display(self):
    print('Child')

# Method Overloading
# Not supported by standard libraries of python
# Can use default values for arguments and work around that
# Otherwise, can use multipledispatch library
from multipledispatch import dispatch
class Operation:
  @dispatch(int, int)
  def add(self, a, b):
    return a + b

  @dispatch(int, int, int)
  def add(self, a, b, c):
    return a + b + c
```

## Encapsulation
```py
class Employee:
  def __init__(self, name):
    self.__name = name

emp = Employee('Tom Cruise')
# Raises error
# Private attributes are encapsulated
emp.__name
```
