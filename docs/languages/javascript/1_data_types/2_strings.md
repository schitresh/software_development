## Strings
- Immutable
  - Cannot be modified in-place once stored in a memory location
  - New characters cannot be appended or inserted
  - Any changes will create a new string object

```js
text = 'hello'
text[1] // 'e'
text + 'world' // 'hello world'
text * 3 // Not supported: NaN

text = "Hello World!"
text = `Hello`
para = `Multiline
String`

'hello'.includes('el') // true

'hello\nworld' // Adds new line
```

## Slicing
```js
// From left, the indexes are 0, 1, 2, ...
// From right, the indexes are ..., -3, -2, -1
// Slicing: string.slice(startIndex, endPosition)
// endPosition = endIndex + 1

text = 'hello world'
text.slice(5) // 'world'
text.slice(-6) // 'world'

text.slice(2, 5) // 'llo'
text.slice(-5, -2) // 'wor'
text.slice(6, -2) // 'wor'
```

## Instance Methods
- Applied on string like `string.methodName(...args)`

```js
length()

// Find and replace
search(strOrRegex) // Returns index if found else -1
indexOf(substr) // Returns index if found else -1
indexOf(substr, startIndex)
// lastIndexOf
replace(strOrRegex, newStr) // replaceAll
includes(subStr)
substring(startIndex)
substring(startIndex, endIndex)

// Prefix & Suffix
startsWith(prefix)
startsWith(prefix, endPosition) // endPosition = endIndex + 1
endsWith(suffix)
endsWith(suffix, endPosition) // endPosition = endIndex + 1

// Split and Join
trim() // Removes trailing whitespaces
// trimStart(), trimEnd()
split(delimiter) // Deletes delimiter, splits and returns list of subsrings
split(delimiter, limit)
array.join(str)

// Case
toLowerCase()
toUpperCase()
```
