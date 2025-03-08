## Debouncing
```js
var output = document.getElementById("output")
var btn = document.getElementById("btn")

// Add event listener to button
btn.addEventListener("click", debounce(function () {
  output.innerHTML = "Hello " + new Date().toLocaleTimeString();
}, 2000));

// Debounce function
function debounce(func, wait) {
  let timeout;

  return function () {
    // Save the context and arguments of the function
    let context = this
    let args = arguments

    clearTimeout(timeout);

    timeout = setTimeout(function () {
        func.apply(context, args);
    }, wait);
  };
}
```
