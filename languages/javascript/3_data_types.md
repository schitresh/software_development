## Data Types
- `null` is used to denote nothing
- `undefined` is used to denote that it's declared but not defined yet
  - So, after declaring `let name`, the value of name will be `undefined`
  - You can specifically assign null if required, `let name = null`
- Categories
  - Primitive: String, Number, Boolean, null, undefined, Bigint, Symbol
  - Composite or Object: Object, Array, Date

## Boolean
```js
// Boolean
flag = true
inProcess = false
```

## Number
```js
// Integer
a = 10

// Floating point number
b = 10.25
```

## String
```js
// String
text = 'Hello World!'
text = "Hello World!"
text = `Hello World`
```

## Array
- Bounded and iterable
- Can store elements with different data types (as opposed to C)

```js
// Array
items = [1, 2.5, 'one', 'two', [3, 4]]
items[2] // 'one'
```

## Object
```js
// Object
word_map = { one: 1, two: 2 }
word_map['one'] // 1
word_map.one // 1

number_map = { 1: 'one', 2: 'two' }
number_map[1] // 'one'
```

## Date
```js
# Date
new Date()
```

## Type Casting
### Implicit Casting
- Also known as coercion
- When a language compiler/interpreter automatically converts an object
- Python is weakly typed
  - Allows automatic type conversion between unrelated data types

```js
// Addition
'10' + 2 = '102'
10 + 2.5 = 12.5
'10' + true = '10true'
1 + true = 2
1 + null = 1
1 + undefined = NaN

// Division
'10' / 5 = 2
'10.1' / 5 = 2.02
'ab' / 2 = NaN

// Subraction
'10' - '5' = 5

// Multiplication
'10' * false = 0
'10' * 2 = 20

// Not
!0 = true
!1 = false
!!0 = true
!'' = true
!!'' = false
!'hello' = false

// Comparision
10 < '5' // false
5 < '10' // true
```

### Explicit Casting
- Using built-in functions to perform explicit conversions

```js
String(100) // '100'
String(10.5) // '10.5'
String([1, 2]) // '1, 2'
String(true) // 'true'
k.toString() // Doesn't work on numbers directly but on variables

Number('100') // 100
Number('10.2') // 10.2
Number('Hello') // Raises error
Number(true) // 1
Number(null) // 0
parseInt('10.2') // 10

Boolean(100) // true
Boolean(0) // false
Boolean(null) // false
Boolean('') // false
Boolean('hi') // true
```
