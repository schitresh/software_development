## State
- Helps components to remember things like user inputs, shopping cart, api data
- Added using 'useState' hook
  - Takes the initial state value and returns the current state and the state setter
  - `const [showInput, setShowInput] = useState(false)`
- State is local & private to each component instance

```js
import { useState } from 'react';

const Gallery = () => {
  const [index, setIndex] = useState(0);

  return (
    <button onClick={() => { setIndex(index + 1); }}>
      Increment
    </button>
  );
}
export default Gallery;
```

### State vs Regular Variables
- Local variables don't persist between renders
  - When react renders a component for second time, it renders from scratch
  - It doesn't consider any changes to the local variables
  - The state variable retains the data between renders
- Changes to local variables don't trigger renders
  - React doesn't realize it needs to render the component again with the new data
  - State setter updates the state variable and triggers re-render

### Working of State
- Consider `const [index, setIndex] = useState(0)`
- When component renders the first time, it will return `[0, setIndex]`
- When user clicks the button, it calls `setIndex(index + 1)`
  - This tells react to remember index is 1 now, and triggers another render
- During the second render, react still sees `useState(0)`
  - But it remembers that index was set to 1
  - So it returns `[1, setIndex]` instead

## Render and Commit
- Before components are displayed on screen, they must be rendered by react
  - Rendering is like taking snapshot of the UI
  - Component function returns a new jsx snapshot while re-rendering
- Triggering a render
  - Initial render
  - Re-render when state of the component (or one of its ancestor) is updated
- Rendering the component
  - For initial render, DOM nodes are created for html tags
  - For re-render, react calculates which properties have changed since previous render
- Committing to the DOM
  - For initial render, react uses appendChild() of DOM API
    - To put all the DOM nodes it has created on screen
  - For re-render, react applies the minimal necessary operations
    - That were calculated while rendering
    - To make the DOM match the latest rendering output
- Browser render or painting
  - After rendering is done and react updates the DOM
  - Browser will repaint the screen

## State Updates
- A state variable's value never changes within a render
  - Even if its event handler's code is aynchronous
- Inside that render's onClick, the state value will continue to be the old value
  - Because react took the snapshot of the UI
- To update state which is an object, spread operator can be used for old values
  - `setPerson(...person, department: 'Finance' )`
- To update state which is an array, do not use in place edits (like push, pop, etc.)
  - Use methods which return a new array (like map, filter, concat, etc.)
  - Or duplicate the array using [...array]

```js
import { useState } from 'react';

const Counter = () => {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        // Initial value: 0
        // After setting value: 5 (in the next render)
        setNumber(number + 5);
        // Value shown by alert: 0
        // The value will remain same for the current render
        // It will be changed in the next render
        setTimeout(() => { alert(number); }, 3000);
      }}>+5</button>
    </>
  )
}
export default Counter;
```

### Consecutive State Updates
- React waits until all code in the event handlers has run before processing state updates
  - This is similar to a waiter taking an order
  - The waiter doesn't run to the kitchen at the mention of your first dish
- This lets you update multiple state variables (even from multiple components)
  - Without triggering too many re-renders
  - This avoids confusing half-finished renders
    - Where only a few state variables have been updated
  - This is also known as batching
- React does not batch across multiple intentional events like clicks
  - Each click is handled separately

```js
import { useState } from 'react';

const Counter = () => {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        // Re-render happens only after all these setNumber() calls
        // Since the state value remains same during a render
        // The value of number will be 0 for all the three calls
        setNumber(number + 1); // 1
        setNumber(number + 1); // 1
        setNumber(number + 1); // 1

        // To set state based on the previous value, updater function can be used
        // React adds that function to its queue
        setNumber((n) => n + 1); // 1
        setNumber((n) => n + 1); // 2
        setNumber((n) => n + 1); // 3
      }}>+3</button>
    </>
  )
}
export default Counter;
```
