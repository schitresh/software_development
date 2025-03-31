## Unnecessary Effects
### Tranforming Data
- You don’t need Effects to transform data for rendering
  - For example, let’s say you want to filter a list before displaying it
  - You might feel tempted to write an Effect to update state when the list changes
- When you update the state
  - React will first call your component functions to get the changes
  - Then commit these changes to the DOM updating the screen
  - Then run your Effects
- If your Effect also updates the state, it restarts the whole process from scratch
  - To avoid the unnecessary render passes
  - Transform all the data at the top level of your components
  - That code will automatically re-run whenever your props or state change

### User Events
- You don’t need Effects to handle user events
  - For example, let’s say you want to send an /api/buy POST request
  - And show a notification when the user buys a product
- In the Buy button click event handler, you know exactly what happened
  - By the time an Effect runs, you don’t know what the user did (which button was clicked)
  - This is why you’ll usually handle user events in the event handlers

## Dependent State
- When something can be calculated from the existing props or state
  - Don’t put it in state, calculate it during rendering instead
- This makes your code
  - Faster as you avoid the extra cascading updates
  - Simpler as you remove some code
  - Less error-prone as you avoid bugs due to state variables getting out of sync
- Suppose you have a component with two state variables: firstName and lastName
  - You want to calculate a fullName by concatenating them
  - Your first instinct might be to add a fullName state and update it in an Effect
- This is more complicated than necessary and inefficient too
  - It does an entire render pass with a stale value for fullName
  - Then immediately re-renders with the updated value

```js
// Avoid: redundant state and unnecessary Effect
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');

  const [fullName, setFullName] = useState('');
  useEffect(() => {
    setFullName(firstName + ' ' + lastName);
  }, [firstName, lastName]);
}

// Good: calculated during rendering
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  const fullName = firstName + ' ' + lastName;
}
```

## Caching Calculations (useMemo)
- If a component receives todos from props and filters them
  - You might feel tempted to store the result in state and update it from an Effect
  - This is both unnecessary and inefficient
- You can filter them while rendering if it is not slow
  - But if you have a lot of todos or the filtering is slow
  - You can cache (or memoize) an expensive calculation by wrapping it in a useMemo Hook
- If dependencies change, useMemo will run the function again and store its result
  - Otherwise it will return the last result it has stored

```js
// Avoid: redundant state and unnecessary Effect
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  const [visibleTodos, setVisibleTodos] = useState([]);

  useEffect(() => {
    setVisibleTodos(getFilteredTodos(todos, filter));
  }, [todos, filter]);
}

// This is fine if getFilteredTodos() is not slow
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  const visibleTodos = getFilteredTodos(todos, filter);
}

// Does not re-run getFilteredTodos() unless todos or filter change
import { useMemo, useState } from 'react';

function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  const visibleTodos = useMemo(() => {
    return getFilteredTodos(todos, filter)
  }, [todos, filter]);
}

// To measure the processing time, you can do this
console.time('filter array');
const visibleTodos = getFilteredTodos(todos, filter);
console.timeEnd('filter array');
```

## Resetting State
- Consider a ProfilePage component having comment input stored in a state variable
  - That does not get reset when you navigate from one profile to another
  - It’s easy to accidentally post a comment on a wrong user’s profile
  - To fix this, you clear the comment state whenever the userId changes
- This is inefficient because
  - ProfilePage and its children will first render with the stale value
  - And then render again
- It is also complicated because
  - You’d need to do this in every component that has some state inside ProfilePage
  - E.g. if the comment UI is nested, you’d want to clear out nested comment state too
- Instead, you can specify that each user’s profile is conceptually a different profile
  - By giving it an explicit key
  - Split your component in two and pass a key from the outer to the inner component

```js
// Avoid: Resetting state on prop change in an Effect
export default function ProfilePage({ userId }) {
  const [comment, setComment] = useState('');
  useEffect(() => { setComment(''); }, [userId]);
}

// State in Profile and below will reset automatically on key change
export default function ProfilePage({ userId }) {
  return <Profile userId={userId} key={userId}/>
}
function Profile({ userId }) {
  const [comment, setComment] = useState('');
}
```

## Adjusting State on Prop Change
- Sometimes you might want to reset or adjust a part of the state on a prop change
  - Consider a List component that maintains the selected item
  - You want to reset the selection whenever the items prop changes
- If we handle this in useEffect, then every time the items change
  - The List and its child components will render with a stale selection value
  - After the useEffect runs, it will re-render the List and its child components
- Instead, adjust the state directly during rendering
  - This will trigger re-render immediately after it exists with a return statement
  - Since the selection value is stale, it skips re-rendering the List children
  - This approach is better but most components shouldn't need this either
- Adjusting state based on props or other state makes your data flow complex
  - Always check whether you can reset all state with a key
  - Or calculate everything during rendering instead
  - So instead of storing (and resetting) the selected item in state variable
    - You can store the selected item ID as regular variable
    - There is no need to adjust the state at all

```js
// Avoid: Adjusting state on prop change in an Effect
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  useEffect(() => { setSelection(null); }, [items]);
}

// Better: Adjust the state while rendering
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  const [prevItems, setPrevItems] = useState(items);
  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  }
}

// Best: Calculate everything during rendering
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selectedId, setSelectedId] = useState(null);

  const selection = items.find((item) => item.id === selectedId) ?? null;
}
```
