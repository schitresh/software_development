## Objects
```js
let subjects = { phy: 'Physics', chem: 'Chemistry', math: 'Mathematics' }
let numbers = { 1: 'one', 2: 'two' } // Converted to string keys: { '1': 'one', ... }

let [a, b, c] = ['hello', 10, {}]
let obj = { a, b, c } // { a: 'hello', b: 10, c: {} }
Object.fromEntries([['a', 100], ['b', 200]]) // { 'a': 100, 'b': 200 }

const student = {
  name: 'John Wick',
  dept: '',
  [`prop${value}`]: 'hello',
  printName: function() { console.log(this.name) },
  printDept() { console.log(this.dept) }
}
student.printName()
student.printDept()

{ ...student1, ...details1 }
```

### Accessing
```js
subjects['phy'] // 'Physics'
subjects.phy // 'Physics'

subjects['math'] = 'Algebra'
subjects.stat = 'Statistics'
delete subjects.math
Object.freeze(subjects)

for (code in subjects) {
  console.log(code)
}

Object.entries(subjects).forEach(([code, name]) => {
  console.log(code, name)
})
```

## Destructuring
```js
const { name, age } = { name: 'Tom Cruise', age: 25, dept: 'Finance' }
const { name: { first, last }, dept } = { name: { first: 'Tom', last: 'Cruise'}, age: 25 }
const { name, ...attributes } = { name: 'Tom Cruise', age: 25, dept: 'Finance' }

// Renaming & Defaults
const { message = 'hello' } = obj
const { message: text } = obj
const { message: text = 'hello' } = obj

// Functions
const sum = ({ num1, num2, num3 }) => num1 + num2 + num3
const { name, dept } = processData()
```

## Instance Methods
```js
Object.keys(obj)
Object.values(obj)
Object.entries(obj) // Array of key-value pairs
Object.groupBy(obj, callbackFn)
```
