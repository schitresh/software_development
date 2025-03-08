## Arithmetic Operators
```js
// Includes regular operators like +, -, *, /
a / b // Float Division: 5 / 2 = 2.5
a % b // Modulus
a ** b // Exponent
a++ // Increment
a-- // Decrement
```

## Logical Operators
```js
a && b
a || b
!a
// Nullish coalescing operator
// Returns b if a is null or undefined
// Otherwise it returns a even if it is false
a ?? b
```

## Membership Operators
```js
a = 10
b = [10, 20]

a in b # True
a in { 10: 'ten' } # True
a not in b # False
```

## Identity Operators
```js
a = [10, 20]
b = [10, 20]
c = a

a is c # True
a is not b # True
```

## Other Operators
- Comparison: ==, !=, ===, !===, >, <, >=, <=
- Assignment: =, +=, -=, *=, /=, etc.
- Bitwise: &, |, ^, ~, <<, >>

## Equality
```js
a == b // Checks only the value
1 == 1 // true
1 == 1.0 // true
1 == '1' // true
1 == true // true
2 == true // false
null == undefined // true

a === b // Checks the value and the data type
1 === 1 // true
1 === 1.0 // true
1 === '1' // false
1 === true // false
2 === true // false
null == undefined // false
```

## Spread Operator
- Spreads the entities into a container

```js
let array = [1, 2, 'hello']
console.log(array) // [1, 2, 'hello']
console.log(...array) // 1 2 hello

[...arr1, ...arr2] // [1, 2, 3, 4] if arr1 = [1, 2] & arr2 = [3, 4]
arr2 = [...arr1] // Clones arr1 to arr2

let words = { two: 4, three: 3 }
{ one: 1, two: 2, ...words } // { one: 1, two: 4, three: 3 }
{ ...words, one: 1, two: 2 } // { one: 1, two: 2, three: 3 }
```

## Rest Operator
- Collects the entities into an array

```js
(a, ...nums) => a + nums.reduce((sum, val) => sum + val, 0)
```
