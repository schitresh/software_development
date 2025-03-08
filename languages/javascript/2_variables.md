## Variables
- Defined using camelcase

```js
// Declaration
let name = 'John Wick'
let employeeName = 'Tom Cruise'
let counter = 100

// Get the data type
typeof(name) // 'string'
typeof(counter) // 'number

delete object.property
delete array[index]
```

## Declaration Types
- Hoisting
  - Moves the declaration of the variables at the top of the code

### Var
- `var name`
- Accessible throughout within the defined function
- Supports hoisting
- Can be redeclared within the same scope (though not a good practice)
- Binds `this`

```js
test = () => {
  if (true) {
    x = 10 // Works because hoisting is supported
    console.log(x) // Prints 10
    var x = 5
    var x = 15
  }
  console.log(x) // Prints 15
}
```

### Let
- `let name`
- Only accessible within the defined block
- Doesn't support hoisting
- Cannot be redeclared within the same scope
- Does not bind `this`
- Was introduced after var
  - To improve scoping behaviors of variables and safety of code

```js
test = () => {
  if (true) {
    // x = 10 raises error as hoisting not supported
    let x = 5
    // let x = 15 raises an error
  }
  // console.log(x) raises an error
}
```

### Const
- `const text = 'hello'`
- Cannot reassign its value
  - Cannot be declared independently: `const text; text = 'hello'` is incorrect
  - Cannot be redeclared within the same scope
  - Though the same object (like array) can be manipulated
- Only accessible within the defined block
- Doesn't support hoisting
- Does not bind `this`
- Was introduced along with let

```js
test = () => {
  if (true) {
    // console.log(x) raises error as hoisting not supported
    const x = 5
    // x = 10 raises an error
    // const x = 15 raises an error
  }
  // console.log(x) raises an error
}
```

## Types
- Local and global variables are determined from the scope they are defined in
- Though global variables are sometimes denoted by uppercase
- No separate declaration types
