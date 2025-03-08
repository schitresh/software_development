## Function
```js
function isEven(number) {
  return number % 2 == 0
}

isEven(4)
```

## Pass by Reference or Value
- Behavior of arguments depends on whether they are mutable or immutable

### Immutable Arguments
- If the passed object is immutable (like numeric), arguments are pass by reference
- Since variable is a reference to the object in memory

```js
function process(value) {
  value = value + 1
  console.log(value) // 11
}

let value = 10
process(value)
console.log(value) // 10
```

### Mutable Arguments
- If the passed object is mutable, arguments are pass by value
- Changing their value creates a new object in memory and leaves the original variable unchanged

```js
function process(array) {
  array.push(3)
  console.log(array) // [1, 2, 3]
}

array = [1, 2]
process(array)
console.log(array) // [1, 2, 3]
```

## Arguments
```js
// Positional arguments
// Required by the function in correct positional order
function process(val1, val2) {
  console.log(val1, val2)
}

// Default arguments
// Default value is used if no value is passed
function process(val1 = 5) {
  console.log(val1)
}

function process(val1 = 5, val2) {
  console.log(val1, val2)
}
// If the default argument occurs at the beginning, we need to specify its value
// Pass undefined to specify to use the default value
process(undefined, 10) // 5 10

// Arbitrary arguments
// Variable number of arguments
// Should be used after positional arguments
function add(...args) {
  return args.reduce((sum, x) => sum + x, 0)
}
add(10, 20, 30, 40)
```

## Function Hoisting
- Function declarations are moved to the top of their local scope before execution
- Default behavior of javascript, though not advisable
- Only declaration is hoisted, not the initialization of its variables

```js
add(10, 20) // 30
function add(a, b) { return a + b }
```
