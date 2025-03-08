## Web API
- Browser API developed on top of the core javascript

## Storage API
- Can be used to store till 5 MB (as opposed to 4 KB per cookie)
- Can be accessed by the client only (cookie by both client & server)
- Fully secured (as opposed to cookie which is vulnerable to stealing)
- localStorage: Never expires
- sessionStorage: expires when the browser tab is closed

```js
// The same methods apply to sessionStorage
localStorage.setItem(key, value)
localStorage.getItem(key)
localStorage.removeItem(key)
localStorage.clear()
```

## Forms API
- Used to perform client-side form validation

```js
// DOM Properties

// Contains multiple properties to perform a particular validation
// customError, valueMissing, rangeOverflow, rangeUnderflow, tooLong
// patternMismatch, stepMismatch, typeMismatch
// valid (boolean to indicate if valid or not)
validity
validationMessage

// DOM Methods
element.checkValidity() // Checks if the input value is valid
element.setCustomValidity(message) // Sets custom message for validation of the input
```

## Worker API
- Allows to run javascript code in a background thread
- Allows to interact with web page without loading the whole JS code
- Increases the response time of the web page
- Used in parallel downloads, background data synchronization
  - Generating reports, processing audio & video
- Cannot use these objects: window, document, parent

```js
<div>
  <button onclick="startWorker()">Start Counter</button>
  <button onclick="stopWorker()">Stop Counter</button>
</div>
<div id="output">
</div>
<script>
  let output = document.getElementById('output')
  let workerObj

  function startWorker() {
    if (typeof WorkerObj !== undefined) return
    const workerObj = new Worker('textWorker.js')
    // To get the message in main thread from the worker file
    // Which is sent using postMessage method, onmessage event is used
    workerObj.onmessage = function (e) {
      output.innerHTML += `Event: ${event.data}<br>`
    }
  }

  function stopWorker() {
    workerObj?.terminate()
    workerObj = undefined
  }

  let = i
  function counter () {
    i += 1
    postMessage(data) // To send data to the main thread
    setTimeout('counter()', 1000)
  }
  counter()
</script>
```

## Fetch API
- Allows a browser to make HTTP request to a web server

```js
// Using Promise
// Options object contains method (GET, PATCH, POST), headers, body, etc.
fetch(URL, options)
  .then(res => res.json())
  .then(data => {
    let output = document.getElementById('output')
    output.innerHTML += JSON.stringify(data)
  })
  .catch(error => {})

// Using Await
let res = await fetch(URL, options)
let data = await res.json()
let output = document.getElementById('output')
output.innerHTML += JSON.stringify(data)
```

## Geolocation API
```js
// Properties
coords
coords.latitude // longitude, altitude, accuracy, altitudeAccuracy, heading, speed


// Methods
// Current geographic location of user
getCurrentPosition(successCallback, errorCallback, options)
// User's live location
watchPosition(successCallback, errorCallback, options)
clearWatch(id) // Clear ongoing watch
```
