## Debouncing
- Debouncing is a simpler way to delay the execution of a particular function
  - Until a certain amount of time has passed since the last execution of the function
- Avoids unnecessary repeated function calls
- For example, when we press the button to call the elevator, it registers the event
  - After that, when you press the call button multiple times in a short period of time
    - It ignores the button press as the elevator can't come faster

```js
var output = document.getElementById("output")
var btn = document.getElementById("btn")

// Add event listener to button
// Waits 2s before printing Hello and time
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
