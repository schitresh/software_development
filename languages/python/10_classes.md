## Instance Methods
```py
class Employee:
  # Constructor
  def __init__(self, name, age):
    # Instance Variables
    self.name = name
    self.age = age

  # Instance Methods
  def display_name(self):
    print(self.name)

Employee.__name__ # Employee
Employee.__module__ # __main__

emp = Employee('John Wick', 25)
emp.name
emp.__dict__ # { 'name': 'John Wick', 'age': 25 }
isinstance(emp, Employee) # True
callable(emp.display_name) # True

hasattr(emp, 'name') # True
getattr(emp, 'name') # 'John Wick'
setattr(emp, 'name', 'Tom Cruise')
delattr(emp, 'name')
```

## Class Methods
```py
class Employee:
  # Class Variables
  department = 'Finance'

  # Class Methods
  @classmethod
  def print_dept(klass):
    print(klass.department)

  @classmethod
  def print_custom_dept(klass, dept):
    print(dept)
```

## Static Methods
- Doesn't require class or self object
- Used for functions that can be standalone but somehow are in context of the class

```py
class Employee:
  # Static Methods

  @staticmethod
  def add(klass, a, b):
    return a + b
```

## Access Modifiers
- Public: name (No underscore)
- Protected: _name (single underscore)
- Private: __name (double underscore)

```py
class Employee:
  def __init__(self, name, age, dept):
    self.name = name # Public
    self._age = age # Protected
    self.__dept = dept # Private

  def display_name(self):
    print(self.name)

# Private attributes can be accessed through Mangling
# But should be refrained from use
emp = Employee('John Wick', 25)
emp._Employee__dept
```

## Getter and Setter
### Property Method
```py
class Employee:
  def __init__(self, name, age):
    self.__name = name
    self.__age = age

  def get_name(self):
    return self.__name

  def set_name(self, name):
    # Can add validations & processing
    self.__name = name

  def delete_name(self, name):
    # Can add validations
    del self.__name

  name = property(get_name, set_name, delete_name)

emp = Employee('John Wick', 25)
emp.name
emp.name = 'Tom Cruise'
del emp.name
```

### Property Decorator
```py
class Employee:
  def __init__(self, name, age, dept):
    self.__name = name
    self.__age = age

  @property
  def name(self):
    return self.__name

  @property.setter
  def name(self, name):
    # Can add validations & processing
    self.__name = name

  @property.deleter
  def name(self, name):
    # Can add validations
    del self.__name

  name = property(get_name, set_name, delete_name)

emp = Employee('John Wick', 25)
emp.name
emp.name = 'Tom Cruise'
del emp.name
```

## Nested Classes
```py
class Student:
  def __init__(self, name):
    self.name = name
    self.subjects = [self.Subject('Physics'), self.Subject('Maths')]

  class Subject:
    def __init__(self, title):
      self.title = title
```
