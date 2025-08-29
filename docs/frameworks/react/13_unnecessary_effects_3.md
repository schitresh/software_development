## Notifying Parent Component
- Let’s say you've a Toggle component with an internal isOn state
  - You want to notify the parent component whenever this state changes
  - So you expose an onChange event and call it from an Effect
- This is not ideal
  - Toggle updates its state and react updates the screen
  - Then React runs the Effect which calls the onChange function
  - Now the parent component will update its own state, starting another render pass
- It would be better to do everything in a single pass
  - Update the state of both components within the same event handler
  - React batches updates from different components together
    - So there will only be one render pass
- You can also 'Lift state up' and let the parent component fully control the Toggle
  - There will be less state overall to worry about
  - When trying to keep two different state variables synchronized, try lifting state up instead

```js
// Avoid: The onChange handler runs too late
function Toggle({ onChange }) {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    onChange(isOn);
  }, [isOn, onChange])

  function handleClick() {
    setIsOn(!isOn);
  }
}

// Good: Perform all updates during the event that caused them
function Toggle({ onChange }) {
  const [isOn, setIsOn] = useState(false);

  function updateToggle(nextIsOn) {
    setIsOn(nextIsOn);
    onChange(nextIsOn);
  }

  function handleClick() {
    updateToggle(!isOn);
  }
}

// Also good: the component is fully controlled by its parent
function Toggle({ isOn, onChange }) {
  function handleClick() {
    onChange(!isOn);
  }
}
```

## Passing Data to Parent
- In React, data flows from the parent components to their children
  - When child components update the state of their parent components in Effects
  - The data flow becomes very difficult to trace
- Since both the child and the parent need the same data
  - Let the parent component fetch that data, and pass it down to the child
  - This is simpler and keeps the data flow predictable

```js
// Avoid: Passing data to the parent in an Effect
function Parent() {
  const [data, setData] = useState(null);
  return <Child onFetched={setData}/>;
}

function Child({ onFetched }) {
  const data = useSomeAPI();
  useEffect(() => {
    if (data) onFetched(data);
  }, [onFetched, data]);
}

// Good: Passing data down to the child
function Parent() {
  const data = useSomeAPI();
  return <Child data={data}/>;
}

function Child({ data }) {
  // ...
}
```

## Subscribing to External Store
- Sometimes your components may need to subscribe to some data outside the React state
  - This could be from a third-party library or a built-in browser API
  - Since this data can change without React’s knowledge
    - You need to manually subscribe your components to it
- Although it’s common to use Effects for this
  - React has a purpose-built Hook for subscribing to an external store
  - This hook is `useSyncExternalStore` which is less error-prone
  - You can write a wrapper Hook like useOnlineStatus() to avoid repeating it

```js
// Not ideal: Manual store subscription in an Effect
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function updateState() {
      setIsOnline(navigator.onLine);
    }

    updateState();

    window.addEventListener('online', updateState);
    window.addEventListener('offline', updateState);

    return () => {
      window.removeEventListener('online', updateState);
      window.removeEventListener('offline', updateState);
    };
  }, []);
  return isOnline;
}

function ChatIndicator() {
  const isOnline = useOnlineStatus();
}

// Good: Subscribing to an external store with a built-in Hook
function subscribe(callback) {
  window.addEventListener('online', callback);
  window.addEventListener('offline', callback);

  return () => {
    window.removeEventListener('online', callback);
    window.removeEventListener('offline', callback);
  };
}

function useOnlineStatus() {
  return useSyncExternalStore(
    subscribe, // React won't resubscribe for as long as you pass the same function
    () => navigator.onLine, // How to get the value on the client
    () => true // How to get the value on the server
  );
}

function ChatIndicator() {
  const isOnline = useOnlineStatus();
}
```

## Fetching data
- Many apps use Effects to kick off data fetching like SearchResults in the example
  - You don’t need to move this fetch to an event handler
  - Because search inputs are often prepopulated from the URL
  - And the user might navigate Back and Forward without touching the input
- It doesn’t matter where page and query come from
  - While the component is visible, you want to keep results synchronized
  - With the data from the network for the current page and query
- However, race conditions need to be handled
  - Typing "hello" fast will query "h", "he", "hel", "hell", "hello" separately
  - But there is no guarantee about the order the responses will arrive in
  - And you might end up displaying the wrong search results
  - To fix this, you need to add a cleanup function to ignore stale responses
- You might also want to think about caching responses
  - So that the user can click Back and see the previous screen instantly
  - How to fetch data on the server
    - So that the initial server-rendered HTML contains the fetched content
    - Instead of a spinner
  - And how to avoid network waterfalls
    - So that a child can fetch data without waiting for every parent
- Modern frameworks provide efficient built-in data fetching mechanisms
  - If you don’t use a framework or build your own
  - Consider extracting your fetching logic into a custom Hook
  - You’ll likely also want to add some logic for error handling

```js
// Avoid: Fetching without cleanup logic
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [page, setPage] = useState(1);

  useEffect(() => {
    fetchResults(query, page).then((json) => {
      setResults(json);
    });
  }, [query, page]);

  function handleNextPageClick() {
    setPage(page + 1);
  }
}

// Good: Ignore stale responses
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [page, setPage] = useState(1);

  useEffect(() => {
    let ignore = false;

    fetchResults(query, page).then((json) => {
      if (!ignore) setResults(json);
    });

    return () => { ignore = true; };
  }, [query, page]);

  function handleNextPageClick() {
    setPage(page + 1);
  }
}

// Good: Extract the logic to a custom hook
function SearchResults({ query }) {
  const [page, setPage] = useState(1);
  const params = new URLSearchParams({ query, page });
  const results = useData(`/api/search?${params}`);

  function handleNextPageClick() {
    setPage(page + 1);
  }
}

function useData(url) {
  const [data, setData] = useState(null);

  useEffect(() => {
    let ignore = false;

    fetch(url)
      .then((response) => response.json())
      .then((json) => {
        if (!ignore) setData(json);
      });

    return () => { ignore = true; };
  }, [url]);

  return data;
}
```
