## Array
- Mutable: Any item can be modified, removed or added
- The elements can be of any data type (unlike C, C++, Java)

```js
let subjects = ['Physics', 'Chemistry', 'Maths']
let items = [25.50, True, -50, 1+2j, 'Physics']
let items = Array(10, 2.5, ...)
let items = Array(2) // Array of length two will undefined values

[1, 2] + [3, 4] // 1, 23, 4
[1, 2].concat([3,4]) // [1, 2, 3, 4]
[5] * 4 // 20
[...Array(5).keys] // [0, 1, 2, 3, 4]
Array.from(Array(5), (x,i) => i * 2) // [0, 2, 4, 6, 8]
Array.from(Array(5), (x,i) => (10 + i) * 2) // [20, 22, 24, 26, 28]
```

### Accessing
```js
subjects[1] // 'Chemistry'
subjects[1] = 'Statistics'
delete subjects[1] // ['Physics', undefined, 'Maths']
[1, 2, 3].includes(2) // true
```

### Destructuring
```js
let [x, y] = [1, 2] // x = 1, y = 2
let [x, ...y] = [1, 2, 3] // x = 1, y = [2, 3]
// let [x, ...y, z] = [1, 2, 3, 4] raises error

const [name, age] = ['Tom Cruise', 25, 'Finance']
const [name, , dept] = ['Tom Cruise', 25, 'Finance']
const [name, [age, dept]] = ['Tom Cruise', [25, 'Finance']]

const [name, ...attributes] = = ['Tom Cruise', 25, 'Finance']
const [message = 'hello'] = [] // Default
[b, a] = [a, b] // Swapping: a & b are already defined

// Functions
const sum = ([num1, num2, num3]) => num1 + num2 + num3
const [name, dept] = processData()
```

## Slicing
```js
// From left, the indexes are 0, 1, 2, ...
// From right, the indexes are ..., -3, -2, -1
// Slicing: array.slice(startIndex, endIndex)

let num = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
num.slice(6) // [7, 8, 9, 10]
num.slice(-4) // [7, 8, 9, 10]

num.slice(2, 5) // [3, 4, 5]
num.slice(-5, -2) // [6, 7, 8]
num.slice(6, -2) // [7, 8]
```

## Instance Methods
```js
// Iterated like iterator.next()
// Which returns { value: [index, item], done: false }
entries() // Iterator with key-value (index-item) pairs
keys() // Interator with key (index)
values() // Interator with values (items)
length()
flat() // [1, [2], [[3, 4]]].flat() returns [1, 2, [3, 4]]
join(str)
// Refer iterators

// Insert
push(...elements) // Add elements at the end
unshift(...elements) // Add elements at the beginning
// Inserts or replaces contents
// splice(2, 0, 10) inserts 10 at index 2
// splice(2, 1, 10, 20) removes 1 item from index 2 and inserts 10, 20
// splice(2, 4, 10, 20) removes 4 items from index 2 and inserts 10, 20
splice(startIndex, deleteCount, ...items) // New array
toSpliced(startIndex, deleteCount, ...items) // In place

// Remove
pop() // Removes last item
pop(index) // Removes item at index
shift() // Removes first item

// Find
// Using iterators: find, findIndex, findLastIndex
count(obj) // Count of occurences of obj
indexOf(obj) // Lowest index where obj occurs
// lastIndexOf

reverse() // In place
toReversed() // New array
sort() // In place
sort(compareFn) // compareFn = (a, b) => { a == b ? 0 : (a < b ? -1 : 1) }
toSorted() // In place
toSorted(compareFun) // In place
```
