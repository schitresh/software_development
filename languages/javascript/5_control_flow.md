## If Else
- Falsy values: 0, false, '', undefined, null, NaN

```js
if (marks < 30) {
  result = 'Failed'
} else if (marks > 75) {
  result = 'Distinction'
} else {
  result = 'Passed'
}

// Inline if
if (marks < 30) result = 'Failed'
marks < 30 ? 'Failed' : 'Passed'

// Short-circuit
marks >= 30 && 'Passed'
```

## Match Case
```js
// Case values can be any literal, even list
switch (category) {
  case 'A':
    result = 'Distinction'
    break
  case 'B':
    result = 'Passed'
    break
  case 'C':
    result = 'Failed'
    break
  default:
    result = 'Unknown'
}
```

## Loops
### For loop
```js
for (let i = 0; i < 10; i++) {
  console.log(i)
}

for (index in words) {
  console.log(words[index])
}

for (key in data) {
  console.log(key)
}

for (let item of words) {
  console.log(item)
}

for (let [key, val] of data) {
  console.log(key, val)
}
```

### While loop
```js
while (i < 10) {
  i += 1
}
```

## Jump Statements
```js
words = ['hello', 'world']

// break
// Will print 'hello'
for (let item of words) {
  if (item === 'world') break
  console.log(item)
}

// continue
// Will print 'world'
for(let item of words) {
  if (item == 'hello') continue
  console.log(item)
}
```
