## Function Context
- In javascript, 'this' is a special keyword that is used within functions
  - To refer to the object on which the function is invoked
- The value of 'this' depends on how a function is defined and called

### Call
- Helps to manipulate 'this' value of a function

```js
// thisObject controls the context for the fucntion
// It is the object whose properties need to be accessed using 'this'
func.call(thisObject, ...args)

function printer() { console.log(this.name) }
const person1 = { name: 'John Wick' }
const person2 = { name: 'Tom Cruise' }
printer.call(person1) // John Wick
printer.call(person2) // Tom Cruise

// Function in object
const student1 = {
  name: 'John Wick',
  printName: function() { console.log(this.name) }
}
const student2 = { name: 'Tom Cruise' }
student1.printName() // John Wick
student1.printName.call(student2) // Tom Cruise
```

### Apply
- Similar to call(), but takes args in an array

```js
func.apply(thisObject, [...args])
```

### Bind
- Helps to manipulate 'this' value of a function without invoking it immediately

```js
// args are optional arguments that will prepended to the arguments
// They will be partially applied when the function is invoked
func.bind(thisObject, ...args)

function printer() { console.log(this.name) }
const person = { name: 'John Wick' }
const printName = printer.bind(person)
printName() // John Wick
```

## Closures
- Allows nested functions to access variables defined in the scope of parent function
  - Even if the execution of the parent function is finished
- Comibination of the function and its lexical environment
  - Allows an inner function to access the outer function scope
- Lexical Scoping
  - Concept in which the scope of the variables is determined at the compilation
  - Based on the structure of the code

```js
function outer(val1) {
  function inner(val2) {
    console.log(val1, val2)
  }
  inner('world')
}
outer('hello') // hello world

function outer(val1) {
  return function inner(val2) {
    console.log(val1, val2)
  }
}
const func = outer('hello')
func('world') // hello world

function setCounter() {
  let counter = 10
  return function count() {
    counter -= 1
    console.log(counter)
  }
}
const func = setCounter()
// Execution of outer function is finished
// Still the nested function can access the outer scope
count() // 9
count() // 8
count() // 7
```
