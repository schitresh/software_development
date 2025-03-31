## This
- Contains reference to an object, useful to access methods & properties
- Represents the context of a function or current code

```js
// Global scope
// Refers to 'window' object
var num = 10
console.log(this.num) // 10
```

## Regular Functions
```js
// this refers the object that the function is accessed on
function print() { console.log(this.name) }
const stu1 = { name: 'stu1' }
const stu2 = { name: 'stu2' }
stu1.print = print
stu2.print = print

stu1.print // stu1
stu2.print // stu2

// Changes based on how function is called
const stu1 = { name: 'stu1', print() { console.log(this.name) } }
const stu2 = { name: 'stu2' }
stu2.print = stu1.print
stu2.print // stu2

// Here the inner function is being called from the global object
// Where name is not defined
// Can use arrow function instead
const student = {
  name: 'Tom Cruise',
  print() {
    return function () { console.log(this.name) }
  }
}
inner = student.print()
inner() // undefined

// In this case, although inner() is inside print() function
// But it is executing on global object
// While print is executing on student object: student.print()
const student = {
  name: 'Tom Cruise',
  dept: 'Finance',
  print() {
    console.log(this.dept)
    function inner() { console.log(this.name) }
    inner()
  }
}
inner = student.print() // Finance, undefined
```

## Arrow Functions
```js
// Cannot be used as methods
const student = {
  name: 'Tom Cruise',
  print1: function() { console.log(this.name) },
  print2: () => { console.log(this.name) }
}
student.print1() // this refers to student, so prints Tom Cruise
student.print2() // this refers to global object, so prints nothing

// Useful in inner functions which are executed on global object
// Using regular function here will print undefined
const student = {
  name: 'Tom Cruise',
  print() {
    const inner = () => { console.log(this.name) }
    inner()
  }
}
student.print() // Tom Cruise

// A class's body has this context
// So arrow functions will correctly point to the instance or static fields
// And because it's a closure and not the function's own binding,
// the value of this won't change based on execution context
class Employee {
  dept = 'Finance'
  printDept = () => { console.log(this.dept) }
}
const emp = new Employee()
emp.printDept() // Finance

const { printDept } = emp
printDept() // Finance
```

## Classes
```js
// Refers to the instance object
class Student {
  constructor(name) {
    this.name = name
    this.dept = dept
  }

  displayName() {
    console.log(this.name)
  }

  displayDept = () => {
    console.log(this.dept)
  }
}
const student = new Student('Tom Cruise', 'Finance')
student.name // Tom Cruise
student.dept // Finance
student.displayName() // Tom Cruise
student.displayDept() // Finance

// In event handler
// this refers to the element on which event is executed
```
