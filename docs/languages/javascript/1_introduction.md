## Characteristics
- Interpreted language
  - No need to compile the program before executing
  - Processed at runtime by interpreter line by line
- Supports functional programming
  - Uses functions as primary building blocks
  - You can define first-class function, pure functions, closures
    - higher order functions, arrow fucntions, function expressions
- Dynamically typed
- Weakly typed
  - Allows automatic type conversion between unrelated data types
  - For example, 1 + '2' is evaluated to '12'
- Automatic garbage collection
- Single threaded programming language
  - Doesn't have multi-threading capabilities
  - To execute code faster, asynchronous programming can be used
- It comes installed on every modern web browser

## Client-side Language
- Helps in manipulating HTML pages
- Used to raise dynamic pop-ups on webpages
- Traps user initiated events like button clicks, link navigation
- Popular libraries are ReactJS, AngularJS, NextJS, VueJS

## Backend-side Language
- Best and most popular library is NodeJS

## Basics
```js
// Single line comment
/*
Multiline comment
*/

// Standard Input
// Opens a dialogue box and takes user input (as a string)
value = prompt('Enter input value')

console.log('Hello') // Prints content in web console
console.log(val1, val2) // Seperated using space by default
document.write('hello') // Writes content directly to web page
alert('hello') // Opens a dialogue box on web page with the given content

// Safe Navigation
person?.info?.name
```

## Strict Mode
- `'use strict';`
- Introduced to make JS code more secure
- Prevents common errors while writing code
  - Like initializing variable without declaration
- Ensures safer code
  - Prevents creation of global variables accidentally
  - Prevents statements like 'with' that can lead to vulnerability
- Prevents using reserved keywords like 'public' which are not implemented yet
  - But may be introduced in future versions
