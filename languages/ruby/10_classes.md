## Instance Methods
```rb
class Employee
  # Constructor
  def initialize(name, age)
    # Instance Variables
    @name = name
    @age = age
  end

  # Instance Methods
  def display_name
    p(name)
  end
end

Employee.name # Employee

emp = Employee.new('John Wick', 25)
emp.display_name
emp.is_a?(Employee) # True
emp.respond_to?(:display_name) # True
```

## Class Methods
```rb
class Employee
  # Class Constant
  COMPANY = 'Company Name'

  # Class Variables
  @@department = 'Finance'

  # Class Methods
  def self.print_dept
    p(@@department)
  end

  class << self
    def print_custom_dept(dept):
      p(dept)
    end
  end
```

## Access Modifiers
```rb
class Animal
  def public_method
    private_method
  end

  protected

    def protected_method
      puts 'protected'
    end

  private

    def private_method
      puts 'private'
    end
end

# Private attributes can be accessed through this
emp = Employee.new('John Wick', 25)
emp.instance_variable_get('@name')
emp.instance_variable_set('@name', 'Tom Cruise')
```

## Getter and Setter
### Usng Regular Methods
```rb
class Employee
  def initialize(name, age)
    @name = name
    @age = age
  end

  # Getter
  def name
    @name
  end

  # Setter
  def name=(value)
    # Can add validations & processing
    @name = value
  end
end

emp = Employee.new('John Wick', 25)
emp.name
emp.name = 'Tom Cruise'
```

### Using Accessors
```rb
class Employee
  attr_accessor :name # Defines getter & setter
  attr_reader :age # Defines getter
  attr_writer :dept # Defines setter

  def initialize(name, age, dept:)
    @name = name
    @age = age
    @dept = dept
  end
end

emp = Employee.new('John Wick', 25, dept: 'Finance')
emp.name
emp.name = 'Tom Cruise'
emp.age
# emp.age = 30 raises error since setter is not defined
# emp.dept raises error since getter is not defined
emp.dept = 'Marketing'
```

## Nested Classes
```rb
class Student
  def initialize(name)
    @name = name
    @subjects = [Subject.new('Physics'), Subject.new('Maths')]
  end

  class Subject
    def initialize(title)
      @title = title
    end
  end
end
```
