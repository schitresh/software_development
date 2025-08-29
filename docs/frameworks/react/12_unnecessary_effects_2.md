## Sharing Logic between Event Handlers
- Let’s say you have a product page with buy and checkout buttons
  - You want to show a notification whenever the user puts a product in the cart
  - Calling showNotification() in click handlers of both buttons feels repetitive
  - So you might be tempted to place this logic in an Effect
- This Effect is unnecessary and can cause bugs
  - Let's say the app remembers the cart between page reloads
  - If you add a product to the cart once and refresh the product's page
    - The notification will appear again
  - This is because product.isInCart will already be true on the page load
    - So the Effect above will call showNotification()
- Ask yourself why this code needs to run
  - Use Effects only for code that should run because the component was displayed
  - Here, the notification should appear because the user pressed the button
    - Not because the page was displayed
  - The shared logic can be in a function and called from both event handlers

```js
// Avoid: Event-specific logic inside an Effect
function ProductPage({ product, addToCart }) {
  useEffect(() => {
    if (product.isInCart) {
      showNotification(`Added ${product.name} to the shopping cart!`);
    }
  }, [product]);

  function handleBuyClick() {
    addToCart(product);
  }

  function handleCheckoutClick() {
    addToCart(product);
    navigateTo('/checkout');
  }
}

// Good: Event-specific logic is called from event handlers
function ProductPage({ product, addToCart }) {
  function buyProduct() {
    addToCart(product);
    showNotification(`Added ${product.name} to the shopping cart!`);
  }

  function handleBuyClick() {
    buyProduct();
  }

  function handleCheckoutClick() {
    buyProduct();
    navigateTo('/checkout');
  }
}
```

## Sending Post Request
```js
// Avoid: Event-specific logic inside an Effect
function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');

  const [jsonToSubmit, setJsonToSubmit] = useState(null);
  useEffect(() => {
    if (jsonToSubmit !== null) post('/api/register', jsonToSubmit);
  }, [jsonToSubmit]);

  function handleSubmit(e) {
    e.preventDefault();
    setJsonToSubmit({ firstName, lastName });
  }
}

// Good: Event-specific logic is in the event handler
function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');

  function handleSubmit(e) {
    e.preventDefault();
    post('/api/register', { firstName, lastName });
  }
}

// Good: Sends analytics because the component was displayed
function Form() {
  useEffect(() => {
    post('/analytics/event', { eventName: 'visit_form' });
  }, []);
}
```

## Chains of Computations
- Sometimes you might feel tempted to chain Effects
  - That each adjust a piece of state based on other state
- It is very inefficient
  - The component (and its children) have to re-render between each set call in the chain
  - As the code evolves, the chain may not be able to fit the new requirements
    - Imagine you are adding a way to step through the game history
    - This will require setting the card state to the values from the past
    - This would trigger the effect chain again and change the data you’re showing
    - Such code is often rigid and fragile
- It’s better to calculate what you can during rendering
  - And adjust the state in the event handler
  - If you implement a way to view game history
    - You can do so without triggering the Effect chain that adjusts every other value
    - If you need to reuse logic, you can extract the event handler function
- In some cases, you can’t calculate the next state directly in the event handler
  - E.g imagine a form with multiple dropdowns
  - Where the options of the next dropdown depend on the selection of the previous dropdown
  - Then, a chain of Effects is appropriate because you are synchronizing with network

```js
// Avoid: Chains of Effects that adjust the state solely to trigger each other
function Game() {
  const [card, setCard] = useState(null);
  const [goldCardCount, setGoldCardCount] = useState(0);
  const [round, setRound] = useState(1);
  const [isGameOver, setIsGameOver] = useState(false);

  useEffect(() => {
    if (card && card.gold) setGoldCardCount((c) => c + 1);
  }, [card]);

  useEffect(() => {
    if (goldCardCount <= 3) return;
    setGoldCardCount(0);
    setRound((r) => r + 1);
  }, [goldCardCount]);

  useEffect(() => {
    if (round > 5) setIsGameOver(true);
  }, [round]);

  useEffect(() => {
    alert('Good game!');
  }, [isGameOver]);

  function handlePlaceCard(nextCard) {
    if (isGameOver) throw Error('Game already ended.');
    else setCard(nextCard);
  }
}

// Good
// Calculate what you can during rendering
// Calculate all the next state in the event handler
function Game() {
  const [card, setCard] = useState(null);
  const [goldCardCount, setGoldCardCount] = useState(0);
  const [round, setRound] = useState(1);

  const isGameOver = round > 5;

  function handlePlaceCard(nextCard) {
    if (isGameOver) throw Error('Game already ended.');

    setCard(nextCard);
    if (!nextCard.gold) return;

    if (goldCardCount <= 3) {
      setGoldCardCount(goldCardCount + 1);
    } else {
      setGoldCardCount(0);
      setRound(round + 1);
      if (round === 5) alert('Good game!');
    }
  }
}
```

## Initializing Application
- Some logic should only run once when the app loads
  - You might be tempted to place it in an Effect in the top-level component
- However, you’ll quickly discover that it runs twice in development
  - Maybe it invalidates the authentication token because it wasn’t designed to be called twice
  - In general, your components should be resilient to being remounted
- Add a top-level variable to track whether it has already executed
- You can also run it during module initialization and before the app renders
  - To avoid slowdown or surprising behavior, don’t overuse this pattern
  - Keep app-wide initialization logic to root component modules like App.js

```js
// Avoid: Effects with logic that should only ever run once
function App() {
  useEffect(() => {
    loadDataFromLocalStorage();
    checkAuthToken();
  }, []);
}

// Only runs once per app load
let didInit = false;

function App() {
  useEffect(() => {
    if (didInit) return;
    didInit = true;
    loadDataFromLocalStorage();
    checkAuthToken();
  }, []);
}

// Only runs once per app load
// Check if we're running in the browser
if (typeof window !== 'undefined') {
  checkAuthToken();
  loadDataFromLocalStorage();
}
function App() {
  // ...
}
```
