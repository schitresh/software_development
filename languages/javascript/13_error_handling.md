## Errors
- Syntax errors
- Logical errors: Output doesn't match the expectation
- Runtime error or exception
  - An event occuring during the execution disrupts the normal flow

```js
throw('error message')
throw new Error('error message') // Error is generic error class
// SyntaxError, ReferenceError, etc.
```

## Try Block
```js
try {
  // Perform operations
} catch (e) {
  // Handle exception
  console.log(e.name)
  console.log(e.message)
} finally {
  // Must execute whether an exception is raised or not
}
```

## Custom Exceptions
```js
class CustomError extends Error {
  constructor(message, status) {
    super(message)
    this.status = status
  }
}

function CustomError(message, name) {
  this.message = message
  this.name
}
CustomError.prototype = Error.prototype
```
