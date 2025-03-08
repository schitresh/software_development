## Classes
```js
// Class Declaration
class Employee {
  constructor(name) {
    this.name = name
  }
}

// Class Expression
class Employee = class {
  constructor(name) {
    this.name = name
  }
}

// Function Class
function Employee(name) {
  this.name = name
}

const emp = new Employee('John Wick')
```

## Instance Methods
```js
class Employee {
  // Instance Variables
  dept = 'Finance'

  // Constructor
  constructor(name, age) {
    // Instance Variables
    this.name = name
    this.age = age
  }

  // Instance Methods
  displayName(this) {
    console.log(this.name)
  }
}

emp = new Employee('John Wick', 25)
emp.name
console.log(emp) // { name: 'John Wick', age: 25 }
```

## Static or Class Methods
- Doesn't require instance of class for execution
- Better performance than regular class methods due to memory optimization

```js
class Employee {
  static dept = 'Finance'

  static displayDept() {
    console.log(this.dept)
  }
}
Employee.displayDept()
```

## Access Modifiers
- Public: name (No underscore)
- Protected: _name (single underscore)
- Private: __name (double underscore)

```js
class Employee {
  // Private attributes need to be declared outside constructor
  #dept
  static #text = 'hello'

  constructor(name, dept) {
    this.name = name // Public Attribute
    this.#dept = dept // Private Attribute
  }

  // Private Method
  #displayDept() {
    console.log(this.#dept)
  }

  // Static Method accessing private attribute
  static displayText() {
    console.log(this.#text)
  }
}
```

## Getter and Setter
```js
class Employee {
  #name
  #age

  constructor(name, age) {
    this.#name = name
    this.#age = age
  }

  get name() {
    return this.#name
  }

  set name(name) {
    // Can add validations & processing
    this.#name = name
  }
}

emp = new Employee('John Wick', 25)
emp.name
emp.name = 'Tom Cruise'
```

## Nested Classes
- Not directly supported
- Can be assigned as attribute, but cannot be used in constructor
