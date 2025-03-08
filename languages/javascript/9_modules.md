## Mixins
- Allows to add properties of other objects or classes
  - Extends the functionality of target object
- Each object contains a built-in property called prototype
  - Prototype is itself an object and will have its own prototype property
  - Prototype chain helps to inherit properties and methods from other objects

```js
Object.assign(target, object)

const target = {
  name: 'parent',
  printMessage() { console.log('hello') }
}

const object = {
  printText() { console.log('world') }
}

Object.assign(target, object)
target.printMessage() // hello
target.printText() // world

// Refer Multiple Inhertiance in OOP Classes
```

## Proxies
- Objects that allow to wrap an object and customize the fundamental operations
  - Like getting and setting object operations
  - Useful to add custom validations and access control
- Also used to implement features like logging, caching, securities

```js
const proxy = new Proxy(target, handler)

const person = { name: 'Tom Cruise', dept: 'Finance' }
const handler = {
  get: (obj, prop) => obj[prop],
  set: (obj, prop, value) => { obj[prop] = value }
}

const personProxy = new Proxy(person, handler)
console.log(personProxy.name)
console.log(personProxy.dept)
```

## Modules
```js
// module.js
export const text = 'hello'
export const printMessage = () => {
  console.log('hello world')
}
class Student {}

// target.js
import { text, printMessage, Student } from './module.js'
printMessage()

// Default Export

// module.js
export const text = 'hello'
export default () => {
  console.log('hello world')
}

// target.js
import printMessage from './module.js'
printMessage()

// Explicit Export

// module.js
const text = 'hello'
const printMessage = () => {
  console.log('hello world')
}
class Student {}

export { text, printMessage }
export default Student

// target.js
import Student from './module.js'
new Student()

// Import all

import * as Shape from './shape.js'
Shape.area()
```
