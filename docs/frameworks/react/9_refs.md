## Refs
- Used when you want a component to remember some information
  - But you don't want that to trigger new renders
- Treat refs as an escape hatch to hold onto values that aren't used for rendering
  - Useful when you work with external systems or browser APIs
  - If much of the application logic and data flow relies on refs, rethink the approach
- Like state, refs are retained by react between re-renders
  - But setting state re-renders a component while changing a ref does not
- Defined like `const ref = useRef(0)`
  - Returns an object like `{ current: 0 }`
  - Current value of ref can be accessed like `ref.current`
  - This value is intentionally mutable (both read & write are possible)
- Ref can point to anything: number, string, object, function
  - It is a plain javascript object with the current property
  - The most common use case is to access a DOM element like `<div ref={myRef}>`

```js
import { useRef } from 'react';

export default function Counter() {
  let ref = useRef(0);

  function handleClick() {
    ref.current = ref.current + 1;
    // Since this is being shown in alert, ref works here
    // Ref can update value and component doesn't have to be re-rendered
    // But if this was being shown in UI (Within button or a separate div)
    // The update value won't be shown because ref doesn't re-render these component
    // In that case, state should be used
    alert('You clicked ' + ref.current + ' times!');
  }

  return (
    <button onClick={handleClick}>
      Click me!
    </button>
  );
}
```

## Ref Workflow
- In react, every update is split in two phases
  - During render, react calls the component to figure what should be on the screen
  - During commit, react applies changes to the DOM
- In general, you don’t want to access refs during rendering
  - During the first render
    - DOM nodes have not been created yet, so ref.current will be null
  - During the rendering of updates
    - DOM nodes haven’t been updated yet, so it’s too early to read them
  - If some side effect is required
    - useEffect should be used to move it out of the rendering calculation
    - It will let react update the screen first, and then run the side effect
- React sets ref.current during the commit
  - Before updating the DOM, react sets the affected ref.current values to null
  - After updating the DOM, react immediately sets them to the corresponding DOM nodes
- Usually, you will access refs from event handlers
  - If you want to do something with a ref, there is no particular event
  - You might need an Effect

```js
import { useState, useRef } from 'react';

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  if (isPlaying) {
    // Calling these while rendering isn't allowed
    // Rendering should be a pure calculation of jsx
    // It should not contain side effects like modifying the DOM
    // Moreover, ref.current will be null because its DOM does not exist yet
    // Or if it's a re-render, ref.current will have the old DOM node
    ref.current.play();
  } else {
    ref.current.pause();
  }

  return <video ref={ref} src={src}/>;
}
```

## State and Ref
- When a piece of information is used for rendering
  - Keep it in state
- When a piece of information is only needed by event handlers
  - And changing it doesn’t require a re-render
  - Using a ref may be more efficient
- You shouldn't read or write the current value of ref during rendering
  - Because changing ref doesn't re-render the component
  - But changes to ref are reflected immediately unlike state
- You can read state at any time
  - But each render has its own snapshot of state which does not change

```js
import { useState, useRef } from 'react';

export default function Stopwatch() {
  const [startTime, setStartTime] = useState(null);
  const [now, setNow] = useState(null);
  const intervalRef = useRef(null);

  function handleStart() {
    setStartTime(Date.now());
    setNow(Date.now());

    clearInterval(intervalRef.current);
    // Set now to current time every 10 milliseconds
    intervalRef.current = setInterval(() => { setNow(Date.now()); }, 10);
  }

  function handleStop() {
    clearInterval(intervalRef.current);
  }

  // Since interval updates now every 10 milliseconds
  // This will re-render and show time elapsed every s10 milliseconds
  let secondsPassed = 0;
  if (startTime != null && now != null) {
    secondsPassed = (now - startTime) / 1000;
  }

  return (
    <>
      <h1>Time passed: {secondsPassed.toFixed(3)}</h1>
      <button onClick={handleStart}>
        Start
      </button>
      <button onClick={handleStop}>
        Stop
      </button>
    </>
  );
}
```

## Manipulting DOM with Refs
- React automatically updates DOM to match your render output
  - So your components won't often need to manipulate it
- But sometimes you need access to DOM elements managed by react
  - For example, to focus a node, scroll to it, or measure its size & position
- If you stick to non-destructive actions like focusing and scrolling
  - You shouldn’t encounter any problems
- However, if you try to modify the DOM manually
  - You can risk conflicting with the changes React is making
  - E.g. forcefully removing a div that is controlled by conditional rendering & state

```js
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef}/>
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

## Accessing another component's DOM nodes
- If you try to put a ref on your own component like `<MyInput ref={inputRef}/>`
  - By default you will get null
  - This is intentional because refs are escape hatch that should be used sparingly
  - Manually manipulating another component's DOM nodes makes your code fragile
- Components that want to expose their DOM nodes have to opt in to that behavior
  - A component can specify that it forwards its ref to one of its children
  - MyInput can use the forwardRef API to achieve this

```js
import { forwardRef, useRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref}/>;
});

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef}/>
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```
