## Inheritance
```rb
class Parent
  def initialize(name)
    @name = name
  end
end

class Child < Parent
  def initialize(name)
    super # Will pass all the arguments
    super(name) # If arguments are different than parent class

    @dept = 'Finance'
  end
end

# Multiple Inheritance
# Not supported, but the behavior can be emulated using mixins
```

## Abstraction
- Can be implemented using inheritance and error handling

```rb
# Abstract Class
class Shape
  def draw
    raise Exception('Not implemented')
  end
end

# Concrete Classes
class Circle < Shape
  def draw
  end
end

class Rectangle < Shape
  def draw
  end
end
```

## Polymorphism
```rb
# Method Overriding
class Parent
  def display
    puts('Parent')
  end
end

class Child < Parent
  def display
    puts('Child')
  end
end

# Method Overloading
# Not supported
```

## Encapsulation
```rb
class Employee
  def initialize(name)
    @name = name
  end
end

emp = Employee.new('Tom Cruise')
# Raises error
# Private attributes are encapsulated
emp.name
```
