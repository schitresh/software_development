# Components
- React is a Javascript library for rendering user interfaces (UI)
  - UI is built from small units like buttons, text, images
  - React lets you combine them into reusable, nestable components
- React applications are built from isolated pieces of UI called components
  - Traditionally, first content was marked up and then interaction was added using js
  - React puts interactivity first while still using the same technology
- React component is a javascript function that you can sprinkle with markup
  - These are UI elements that can be reused
  - They use the same html tags under the hood
  - Can be as small as button or as large as an entire page
- Defining components
  - Their names must start with a capital letter
  - Should return jsx which should be rendered on the screen
    - If nothing should be rendered, it should return null
  - Multiple components can be defined in the same file
  - Components can be nested & rendered but their definitions cannot be nested

```js
const Profile = () => (
  <img src='image_url'/>
);

const Gallery = () => (
  <div>
    <Profile/>
    <Profile/>
  </div>
);
```

## JSX
- Syntax extension that is written like html
- But actually it is javascript under the hood
- Lets you embed markup inside javascript
- Variables or logical statements are enclosed within `{}`

### JSX Rules
- Return a single root element
  - If there are multiple elements, wrap them with a single parent tag
  - If you don't want to use an extra element like `div`, fragment can be used
  - Fragment is an empty tag & lets you group things `<React.Fragment></React.Fragment>`
- Close all the tags
  - Self-closing tags like `img` work in html: `<img src=''>`
  - But they should be closed in jsx: `<img src=''/>`
  - Wrapping tags like `li` should also have a closing tag: `<li>text</li>`
- camelCase most of the things
  - Attributes cannot contain dashes like `margin-left`: `<img style={{ marginLeft: 2 }}/>`
  - Reserved words like `class` cannot be used: `<img className={classes.image}/>` -->

```js
const TodoList = () => {
  const person = { name: 'Name', theme: {} };

  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img className='avatar' src='image_url'/>
      <ul>
        <li>Task 1</li>
        <li>Task 2</li>
      </ul>
    </div>
  );
}
export default TodoList;
```
<!--
## Props
- React components use props to communicate with each other
- Similar to html attributes, but you can pass any javascript value through props
  - Including objects, arrays, functions, jsx
  - Props can be destructured in the function arguments of the component
- Props are read-only and immutable
  - If props need to be changed, the parent component can pass new values
  - Hence props are not always static, they can be updated by parent
- Every parent component can pass some information
  - To its child components by giving them props
- When you nest content inside a jsx tag
  - The parent conmponent will receive that content in a prop called children
  - In `<Card><Avatar/></Card>`, Card component will receive Avatar in 'children' prop

```js
const Profile = () => (
  <Card>
    <Avatar
      size={100}
      person={{ name: 'Katsuko Saruhashi', image_url: 'image_url' }}
    />
  </Card>
);

const Avatar = ({ person, size }) => (
  <img
    className='avatar'
    src={person.image_url}
    alt={person.name}
    width={size}
    height={size}
  />
);

const Card = ({ children }) => (
  <div className="card">
    {children}
  </div>
);
```

## Rendering Lists
- Javascript's `map()` can be used to transform
  - An array of data into an array of components
- For each mapped component, a unique 'key' should be specified as prop
  - Keys tell react which item corresponds to which component
    - And helps in selective re-rendering
  - Avoid using index as the key because array indexes can change for specific items
    - Best key is anything that identifies a data item uniquely
    - If no key is provided, react will use the indexes anyway
  - If the key changes, the component will be re-rendered
    - So the keys must not change unless custom re-rendering is required

```js
const List = ({ people }) => (
  <div>
    {people.map((person) => (
      <li key={person.id}>
        <b>{person.name}</b>
        <p>{person.description}</p>
      </li>
    ))}
  </div>
)
export default List;
```

## UI as Tree
- React uses trees to model relationships between components and modules
- This also helps in understanding dependencies of components
- Root node of the tree is the root component of the app

## Hooks
- Special functions that are only available while react is rendering
  - They let you hook into different react features
  - Can be thought of as unconditional declarations about the component's needs
- Can only be called at the top level of components
  - Cannot be called inside conditions, loop or other nested functions
- Example: useState, useContext, useEffect, etc.
