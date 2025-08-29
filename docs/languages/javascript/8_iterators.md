## Iterators
```js
array.every((x) => x % 2 == 0) // Equivalent to all?
array.some((x) => x % 2 == 0) // Equivalent to any?

// Performs operation on each item and returns the original array
array.forEach((x) => console.log(x))

// Manipulates each item and returns the new values of array
array.map((x) => x * 2)

// Selects the items and returns a new array with those items
array.filter((x) => x % 2 == 0)

// Finds and returns the first matched value
array.find((x) => x = 'hello')
// findIndex, findLast, findLastIndex

// Aggregates the values into a variable
// Need to return the aggregator
array.reduce((sum, x) => sum += x, 0)
array.reduce((sum, x) => sum += x, 5) // initial value of sum will be 5
```
