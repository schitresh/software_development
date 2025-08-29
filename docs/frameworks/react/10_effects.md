## Effect
- Some components need to synchronize with external systems
  - Control a non-react component based on react state
  - Set up a server connection
  - Send an analytics log when a component appears on the screen
- Effects let you run some code after rendering
  - So that component can be synchronized with some system outside of react
  - Like network or a third party library
- Effects let you specify side effects that are caused by rendering itself
  - Rather than by a particular event
  - Like a chatroom component that must connect to chat server whenever it's visible

### Application
- Rendering should be a pure calculation of jsx
- It should not contain side effects like modifying the DOM
- So using ref won't work
  - During first render, ref.current will be null because its DOM does not exist yet
  - During re-render, ref.current will have the old DOM node
- By wrapping the DOM update in an Effect
  - You let React update the screen first and then run your side effect

## Working
- Effects run at the end of a commit after the screen updates
  - By default, they run after every render
- But they should only run when needed, this can be achieved by specifying dependencies
  - If dependencies are not handled properly, it might produce an infinite loop
  - E.g. you don't want to re-connect to the server after every keystoke

```js
// This will create an infinite loop
// Setting state triggers rendering and rendering triggers useEffect
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1);
});

useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount and if either a or b have changed since the last render
}, [a, b]);
```

## Re-runs and Unmounts
- Imagine a component (ChatRoom) is a part of a larger app with many different screens
  - The user starts their journey on the ChatRoom page
  - The component mounts and calls connection.connect()
- Then imagine the user navigates to another screen, like Settings page
  - The ChatRoom component unmounts
- Finally, the user clicks Back and ChatRoom mounts again
  - This would set up a second connection
  - But the first connection was never destroyed!
  - As the user navigates across the app, the connections would keep piling up
- To handle this, a cleanup function can be added
  - React will call the cleanup function each time before the effect runs again
  - And when the component unmounts
- Bugs like this are easy to miss without extensive manual testing
  - To help you spot them quickly in development
  - React remounts every component once immediately after its initial mount
  - So useEffect will essentially run twice in development
  - Turning off strict mode will opt out of this development behavior
- In the example below
  - Without cleanup function, it will log 'Connecting', 'Connecting'
  - With cleanup function, it will log 'Connecting', 'Disconnected', 'Connecting'

```js
import { useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    // Cleanup function
    return () => connection.disconnect();
  }, []);
}

export function createConnection() {
  return {
    connect() { console.log('Connecting'); },
    disconnect() { console.log('Disconnected'); }
  };
}
```

## Cleanup Functions
- If remounting breaks the logic of the app, it usually uncovers existing bugs
- Effects are fired twice in development mode to catch these bugs

```js
// Cleanup not required since the value of zoomLevel will remain the same
// And hence useEffect won't be executed again
useEffect(() => {
  mapRef.current.setZoomLevel(zoomLevel);
}, [zoomLevel]);

// Cleanup is required or else showModel will execute twice
useEffect(() => {
  dialogRef.current.showModal();
  return () => dialog.close();
}, []);

// Subscribing to events: Cleanup function should unsubscribe
useEffect(() => {
  window.addEventListener('scroll', handleScroll);
  return () => window.removeEventListener('scroll', handleScroll);
}, []);

// Fetching data
// Cleanup function should either abort the fetch or ignore its result
// Network request that already happened can't be undone, but it should not affect the app
// Hence if the userId changes, the old response should be ignored
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) setTodos(json);
  }
  startFetching();

  return () => { ignore = true; };
}, [userId]);
```
