## Event Handler
- Functions passed to events like onClick, onMouseEnter, etc.
- These functions should be passed, not called
  - Calling will execute the function inline
- They can be passed as props as well
  - The names can be customized like 'onSmash', 'onPlay' since they are props
  - This allows to execute different handlers on different conditions or components

```js
const Button = () => {
  const handleClick = () => {
    alert('Button is clicked!');
  }

  return (
    <button onClick={handleClick}>
      Click
    </button>
  );
}
export default Button;
```

## Event Propogation
- Event handlers will also catch events from any children components
- Event starts with where it happened and propogates or bubbles up the tree

```js
// Case 1: When the button is clicked,
// It will first display 'Button' alert, and then 'Toolbar' alert
// Case 2: When the toolbar is clicked
// It will display only the 'Toolbar' alert
const Toolbar = () => (
  <div className="Toolbar" onClick={() => alert('Toolbar')}>
    <button onClick={() => alert('Button')}>
      Click
    </button>
  </div>
);
export default Toolbar;
```

## Stop Propogation
- To prevent an event from reaching the parent component
  - `event.stopPropagation()` can be called on the event object

```js
// When the button is clicked, only 'Button' alert is displayed
const Toolbar = () => (
  <div className="Toolbar" onClick={() => alert('Toolbar')}>
    <Button onClick={() => alert('Button')}>
      Click
    </Button>
  </div>
);

const Button = ({ onClick, children }) => {
  return (
    <button onClick={(event) => {
      event.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}
```

## Prevent Default
- Some browser events have default behavior associated with them
- For example, a 'form' submit event reloads the whole page by default
- To stop this, `e.preventDefault()` can be called on the event object

```js
const Signup = () => (
  <form onSubmit={(event) => {
    event.preventDefault();
    alert('Submitting!')
  }}>
    <input/>
    <button>Send</button>
  </form>
);
export default Signup;
```
