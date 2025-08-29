## Lifecycle of Effect
- Effects have a different lifecycle from components
- Components goes through the same lifecycle
  - Mount when added to the screen
  - Update when it receives new props or state (usually in response to an interaction)
  - Unmount when removed from the screen
- An Effect can only do two things
  - To start synchronizing something
    - Runs after first render and after re-renders if dependencies changes
    - Dependency values (old & new) are compared with `Object.is`
  - To stop synchronizing it later (cleanup function)
    - Runs before useEffect when component re-renders
    - Or if the component unmounts
- This cycle can happen multiple times
  - If your Effect depends on props and state that change over time
  - Check that you’ve specified your Effect’s dependencies correctly
  - This keeps your Effect synchronized to the latest props and state

## Single or Multiple Effects
- Each Effect in your code
  - Should represent a separate and independent synchronization process
- Consider a ChatRoom component
  - It should connect to the server depending on roomId
  - And it should send an analytics log depending on roomId
- You might be tempted to add both in a single useEffect since the dependency is same
  - But these two logics are different which may change in future
  - Or the dependencies may change later
  - So declare two separate useEffect even if the dependency (roomId) is same here
- On the other hand, if you split up a cohesive piece of logic into separate Effects
  - The code may look cleaner but will be more difficult to maintain

## Dependencies
- It is important to include dependencies on which useEffect logic depends
  - Especially states and props
  - Values calculated from the props & state are also reactive
  - Essentially anything that can be re-calculated during a render
- Constants and variables that do not change are allowed but can be excluded
  - Url constant declared at the top won't change
  - State setter functions don't change
  - Mutable values like `location.pathname`
    - They can be changed at any time completely outside rendering data flow
    - Changing it wouldn't trigger a re-render of the component
    - Even if specified in dependencies
      - React won't know to re-synchronize the Effect when it changes
  - Refs because `ref.current` is mutable and changing it doesn't trigger a re-render
- Check if constants (declarations) can be moved outside the component
  - Or if variables (declarations) can be moved inside useEffect so that they're not reactive

### Objects and Functions as Dependencies
- Avoid relying on objects and functions as dependencies
  - They will be different on every render
  - In JavaScript, each newly created object and function is considered distinct
  - It doesn’t matter that the contents inside of them may be the same
- Either move them outside the component or inside useEffect
- You can also deconstruct objects or use return value of functions
  - And use specific values as the dependent attributes

```js
// Bad: Using object or function as dependency
// Comparing options object between renders will always return true
// Even if nothing is changed: Object.is(options1, options2) will return true
function ChatRoom({ options }) {
  const [message, setMessage] = useState('');

  useEffect(() => {
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [options]);
}

// Good: Using specific attributes as dependency
function ChatRoom({ options }) {
  const [message, setMessage] = useState('');

  const { roomId, serverUrl } = options;
  // Or if function is used
  // const { roomId, serverUrl } = getOptions();
  useEffect(() => {
    const connection = createConnection({
      roomId: roomId,
      serverUrl: serverUrl
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, serverUrl]);
}
```

## Avoiding Redundant Renders
- This Effect updates the messages state every time a new message arrives
- However, since messages is a reactive value read by an Effect, it must be a dependency
  - But making messages a dependency introduces a problem
  - Every time you receive a message, setMessages() re-renders the component
  - So every new message will make the chat re-connect
- To fix the issue, don’t read messages inside the Effect
  - Instead, pass an updater function to setMessages
  - React puts your updater function in a queue and provides the msgs argument
  - This is why the Effect itself doesn’t need to depend on messages anymore
  - And receiving a chat message will no longer make the chat re-connect

```js
// Bad: Dependency on messages is not declared
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    const connection = createConnection();
    connection.connect();

    connection.on('message', (receivedMessage) => {
      setMessages([...messages, receivedMessage]);
    });
  }, [roomId]);
}

// Bad: Dependency on messages is declared
// But chat will re-connect every time a new message is received
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    const connection = createConnection();
    connection.connect();

    connection.on('message', (receivedMessage) => {
      setMessages([...messages, receivedMessage]);
    });

    return () => connection.disconnect();
  }, [roomId, messages]);
}

// Good: Chat won't reconnect every time a new message is received
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    const connection = createConnection();
    connection.connect();

    connection.on('message', (receivedMessage) => {
      setMessages((msgs) => [...msgs, receivedMessage]);
    });

    return () => connection.disconnect();
  }, [roomId]);
}
```
