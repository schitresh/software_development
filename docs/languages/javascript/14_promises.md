## Promise
- Object representing the eventual completion or failure of async operation
- Returned object on which callbacks are attached instead of passing them
- It is in one of these states: pending, fulfilled, rejected

```js
function successCallback(result) {}
function failureCallback(error) {}

// Without Promise: Passing callbacks
asyncOperation(data, successCallback, failureCallback)
// With Promise: Attaching callbacks
asyncOperation(data).then(successCallback, failureCallback)
```

## Chaining
- Passing callbacks can become complicated for chained async operations

```js
// Without Promise
operation1(function(result1) {
  operation2(result2, function(result2) {
    operation3(result3, function(result3) {
      // Processing
    }, failureCallback)
  }, failureCallback)
}, failureCallback)

// With Promise
operation1()
  .then(function(result1) {})
  .then(function(result2) {})
  .then(function(result2) {})
  .catch(failureCallback)
  .finally(function() {})

operation()
  .then((url) => fetch(url))
  .then((res) => res.json())
  .then((data) => { dataArray.push(data) })
  .then(() => { console.log(dataArray) })
  .catch(failureCallback)
  .finally(() => {})
```

## Asyc and Await
```js
async function doSometing() {
  try {
    const url = await operation()
    const res = await fetch(url)
    const data = await res.json()
    dataArray.push(data)
    console.log(dataArray)
  } catch (e) {
    failureCallback(e)
  }
}
```

## Creating Promises
```js
new Promise((resolve, reject) => {
  try {
    value = doSomething()
    resolve(value)
  } catch (e) {
    reject(e.message)
  }
})
```

## Then
```js
then(onFulfilled)
then(onFulfilled, onRejected)

promise
  .then((value) => {
    console.log(`Success: ${value}`)
  })

promise
  .then(
    (value) => { console.log(`Success: ${value}`) },
    (reason) => { console.log(`Failure: ${reason}`) }
  )
```
