## Preserving and Resetting State
- React builds render trees for the component structure in your UI
- When you give state to a component, you might think it lives inside the component
  - But the state is actually held inside React
  - React associates each piece of state it’s holding with the correct component
  - By where that component sits in the render tree
- React will keep the state around as long as you render
  - The same component at the same position in the tree
  - If it gets removed or a different component gets rendered at the same position
    - React discards its state
- State behavior
  - Two identical components rendered at two different places
    - Will preserve individual states
  - Two identical components rendered at the same place
    - Will preserve the same state
    - This happens commonly in conditional rendering
  - Two different components rendered at the same place (conditional rendering)
    - Will destroy & reset the state when the component is switched

```js
const App = () => {
  const counter = <Counter/>;
  const [isFancy, setIsFancy] = useState(0);
  return (
    <div>
      {/* These are two separate counters */}
      {/* Because each is rendered at its own position in the tree */}
      {/* Each of them will get its own independent score state */}
      {counter}
      {counter}

      {/* These counters are rendered at the same position in the tree */}
      {/* So this will preserve the score state */}
      {/* Toggling isFancy state won't change the value of the score */}
      {isFancy ? <Counter isFancy={true}/> : <Counter isFancy={false}/>}

      {/* Toggling isPaused or isFancy will reset the state of the counter */}
      {/* The structure should be same to preserve the state */}
      {isPaused ? <p>See you later!</p> : <Counter/>}
      {isFancy ? (
        <div> <Counter isFancy={true}/> </div>
      ) : (
        <section> <Counter isFancy={false}/> </section>
      )}
    </div>
  );
}
export default App;

const Counter = () => {
  const [score, setScore] = useState(0);
  return (
    <div>
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

## Avoid Nested Declarations
```js
const MyComponent = () => {
  const [counter, setCounter] = useState(0);

  const MyTextField = () => {
    const [text, setText] = useState('');
    return <input value={text} onChange={(e) => setText(e.target.value)}/>
  }

  return (
    <>
      {/* Every time the button is clicked, MyTextField will re-render */}
      {/* and the input will disappear. This is because the function MyTextField */}
      {/* is created for every render. Hence, always declare component functions */}
      {/* at the top level and don't nest their definitions */}
      <MyTextField/>
      <button onClick={() => { setCounter(counter + 1) }}>
        Clicked {counter} times
      </button>
    </>
  );
}
export default MyComponent;
```

## Deliberate State Reset
- By default, react preserves state of a component while it stays at the same position
- But sometimes, you may want to reset a component’s state
- There are two ways to reset state and switching components
  - Render components in different positions
    - Not scalable if there are many components
  - Give each component an explicit identity with key
    - Keys let you tell React that this is not just a first counter
      - Or a second counter, but a specific counter
    - Note that keys are not globally unique
      - They only specify the position within the parent

```js
const Scoreboard = () => {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {/* Initially isPlayerA is true, so the first position contains Counter state */}
      {/* When the button is clicked, the first position clears and state is destroyed */}
      {isPlayerA && <Counter person="Taylor"/>}
      {/* Initially isPlayerA is true, so the seconds position is empty */}
      {!isPlayerA && <Counter person="Sarah"/>}

      {/* Specifying a key tells React to use the key itself as part of the position */}
      {/* instead of their order within the parent */}
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor"/>
      ) : (
        <Counter key="Sarah" person="Sarah"/>
      )}

      <button onClick={() => { setIsPlayerA(!isPlayerA); }}>
        Next player!
      </button>
    </div>
  );
}
export default Scoreboard;
```
