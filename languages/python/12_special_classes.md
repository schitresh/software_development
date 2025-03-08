## Singleton Class
```py
class Test:
  _instance = None

  def __new__(klass):
    if klass._instance is None:
      print('yes')
      klass.instance = super(Test, klass).__new__(klass)

    return klass._instance
```

## Wrapper Class
```py
def test_decorator(Wrapped):
  class Wrapper:
    def __init__(self, name):
      self.wrap = Wrapped(name)

    def print_name(self):
      print(self.wrap.name)

  return Wrapper

# Equivalent to calling `test_decorator(Wrapped)`
@test_decorator
class Wrapped:
  def __init__(self, name):
    self.name = name

obj = Wrapped('Tom Cruise')
obj.print_name()
```

## Enum
```py
from enum import Enum

class subjects(Enum):
  PHYSICS = 1
  MATHS = 2

# Multiple members can be assigned same value
# Adding @unique here will prohibit that and raise error
@unique
class subjects(Enum):
  PHYSICS = 1
  MATHS = 1

# String based
class subjects(Enum):
  PHYSICS = 'P'
  MATHS = 'M'

maths = subjects.MATHS
maths.name # MATHS
maths.value # M

for subject in subjects:
  print(subject.name, subject.value)
```
