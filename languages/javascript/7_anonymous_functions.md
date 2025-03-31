## Function Expression
- Doesn't support hoisting

```js
// Calling add(10, 20) before declaration will raise error since hoisting is not supported
const add = function (a, b) { return a + b }
add(10, 20)

const student = {
  name: 'John Wick',
  printName: function() { console.log(this.name) }
}
student.printName() // John Wick

// Self invoking function
// All variables in such functions are private, limited to the function scope
(function(text) {
  console.log(text)
})('hello')
```

## Arrow Function
- Doesn't have its own binding to 'this', 'arguments' or 'super'
- Should only be used for non-method functions
  - Because arrow functions don't have their own 'this'
  - Refer 'this' keyword topic

```js
const printer = (text = 'hello') => { console.log(text) }
const add = (a, b) => { return a + b }
const add = (a, b) => a + b // Directly returns the value
const data = (text) => ({ message: text }) // Directly returns the hash
```
