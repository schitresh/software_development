## Inheritance
```js
class Parent {
  constructor(name) {
    this.name = name
  }
}

class Child extends Parent {
  constructor(name) {
    super(name)
    this.dept = 'Finance'
  }
}

class AnotherChild extends Parent {
  constructor(dept, name) {
    super(name)
    this.dept = dept
  }
}

parent = new Parent('John Wick')

// Multiple Inheritance
// Not Supported, can be emulated using mixins
// This is because each object can have only one prototype in javascript
// Prototype is a property that allows to share methods across all instances

// Multiple Inheritance Using Object
const student = {
  class: 'X',
  printText() { console.log('hello' ) }
}
Object.assign(Child.prototype, student)
Object.assign(Child.prototype, student, profile, ...)

child = new Child('John Wick')
child.class // X
child.printText() // Hello

// Multiple Inheritance Using Class
class Entity {
  hello () { console.log('Hello') }
}

class Human extends Entity {
  walk () { console.log('Walk') }
}

function Athelete(Parent) {
  return class extends Parent {
    exercise () { console.log('Exercise') }
  }
}
class Swimmer extends Athelete(Human) {}

const swimmer = new Swimmer()
swimmer.hello()
swimmer.walk()
swimmer.exercise()
```

## Abstraction
- No specific abstract class or method
- Can be implemented using inheritance and error handling

```js
class Shape {
  draw () {
    throw new Error('Not implemented')
  }
}

// Concrete Classes
class Circle extends Shape {
  draw () {
  }
}

class Rectange extends Shape {
  draw () {
  }
}
```

## Polymorphism
```js
// Method Overriding
class Parent {
  display () {
    console.log('Parent')
  }
}

class Child extends Parent {
  display () {
    console.log('Child')
  }
}

class AnotherChild extends Parent {
  display (text) {
    super.display() // To call parent method if required
    console.log(text)
  }
}

// Method Overloading
// Not supported
```

## Encapsulation
```js
class Employee {
  #name

  constructor(name) {
    this.#name = name
  }
}

emp = Employee('Tom Cruise')
// Raises error
// Private attributes are encapsulated
emp.name
emp.#name

// Can also be achieved using nested functions
// Scope of outer function is accessible to inner function only
```
