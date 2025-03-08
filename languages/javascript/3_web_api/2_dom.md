## Browser Object Model (BOM)
- Refers to the objects provided by the browsers to interact with them
- By using these objects, browser functionality can be manipulated
  - For example, browser history, window size, navigate to urls
- It is not standardized and depends on the browser in use

## Document Object Model (DOM)
- HTML DOM allows javascript to access and modify the content of HTML elements
- It is a subset of Browser Object Model (BOM)
  - That is standardized and is specific to the current HTML document
  - Mainly focuses on the structure of the displayed document
- Defines the logical or tree-like structure of the document
  - In the tree, each branch ends in a node and each node contains objects
  - DOM methods allows programmatic access to the tree
  - And allows to change the document's structure, content or style
- Javascript can't interact with HTML elements directly, so it creates a DOM

## Window Object
- Current browser window
- Can be used to show alerts, opening new window, closing current window
- All global variables and functions belong to the window object
- All the other objects like document, screen, history belong to the window object

```js
// Properties
name
frames // Gets window items like iframes
frameElement
innerWidth
innerHeight
outerWidth
outerHeight
screenX // Current X coordinate in pixels
screenY // Current U coordinate in pixels
scrollX // Number of pixels scrolled horizontally
scrollY // Number of pixels scrolled vertically

// Methods
print()
getSelection() // Returns selected object
alert(message)
confirm(message) // Show confirm box to get confirmation from users
prompt(message) // Show prompt bos to get user input

focus()
moveBy(deltaX, deltaY) // Changes position of window relative to current position
moveTo(x, y) // Changes position of window absolutely
scrollBy(deltaX, deltaY) // Relative scroll
scrollTo(x, y) // Absolute scroll
open() // Opens a new window
open(url, target)
close() // Closes the current window
```

## Document Object
- Currently opened web page in the browser window
- Forms the root node of the html document called HTML DOM
- Accessed as `window.document` or just `document`
- Provides properties and methods to access HTML elements & manipulate them

```js
// Properties
title // Get or set title of the document
URL
documentElement // <html>
head // <head>
body // <body>
forms // all <forms>
children
cookie


// Methods
getElementById(id)
getElementById(class)
getElementByName(name)
getElementByTagName(tagName)
querySelector(cssSelector) // #test, div.note, input, p, .highlighted
querySelectorAll(cssSelector)

createElement(tagName)
createTextNode(text)
createAttribute(name)
getAttribute(name)
setAttribute(name)
appendChild(node)
removeChild(node)
addEventListener(eventType, callbackFn)
removeEventListener()
```

### Cookies
```js
const setCookie = (key, value, expireInDays) => {
  let cookie = `${key}=${value};`

  if (expireInDays != null) {
    const expireInSeconds = days * 24 * 60 * 60
    let expiryDate = newDate()
    expiryDate.setTime(date.getTime() + expireInSeconds * 1000)
    expiryDate = expiryDate.toUTCString()
    cookie += `exipres=${expiryDate}`
  }

  document.cookie = cookie
}

const deleteCookie = (key) => {
  setCookie(key, null, 0)
}

const getCookie = (cookieKey) => {
  // Decode the cookies to remove any encoding traces
  let cookies = decodeURIComponent(document.cookie);
  cookies = cookies.split(';');

  for (let cookie of cookies) {
    let [key, value] = cookie.split('=');
    key = key.trim();
    if (key === cookieKey) return value.trim();
  }
}
```

## Screen Object
- Window screen of the user device
- Accessed as `window.screen` or just `screen`

```js
// Properties
width
height

// Methods
```

## History
- History of current session of the window
- Accessed as `window.history` or just `history`

```js
// Properties

// Methods
back()
forward()
```

## Navigator
- Used to get browser name, version, cookies
- Accessed as `window.navigator` or just `navigator`

```js
// Properties
appName
appVersion
cookieEnabled
platform // Platform or OS
userAgent // user-agent header
```

## Location
- Helps to manipulate information of the window location (URL)
  - For example to get the host from current url
- Accessed as `window.location` or just `location`
- Set `window.location = url` to go or redirect to a specific url

```js
// Properties
hash // Get or set anchor part of URL
host // Get or set hostname or port number of URL
hostname
href // Get or set URL of current window
origin // Protocol, domain, and port of URL
pathname // Path or route
port
protocol
search // Get or set query string of URL

// Methods
assign(url) // Loads new document at given URL
replace(url) // Replaces the current document with a new document at given URL
reload()
toString() // URL in string format
```

## Console
- Allows to access the debugging console of the window
- Accessed as `window.console` or just `console`

```js
// Methods
log(message)
info(message)
warn(message)
error(message)
clear()
```
