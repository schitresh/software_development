## Context
- Usually information is passed from parent to child component via props
  - But it can become verbose and inconvenient if there are many middle components
  - Or if many components need the same information
  - This can lead to a situation called prop drilling
- Context lets the parent component make information available
  - To any component in the tree below without explicitly through props
- Before using context
  - Start by passing props and see if context is really required
  - Use jsx as children to reduce the number of layers
    - E.g. let's say you pass data props to visual components that don't use them directly
      - Like this: <Layout posts={posts}/>
      - Use this instead: <Layout><Posts posts={posts}/></Layout>
- Uses cases for context
  - Theming: Styling & appearance (e.g. dark mode)
  - Currently logged in user
  - Routing: So that every link knows whether it's active or not
  - Managing state: Many distant components may want to change the state

```js
// LevelContext.js
import { createContext } from 'react';

export const LevelContext = createContext(0);

// Section.js
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

const Section = ({ children }) => {
  const level = useContext(LevelContext);
  return (
    <section className="section">
      <LevelContext.Provider value={level + 1}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}
export default Section;

// Header.js
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

const Heading = ({ children }) => {
  const level = useContext(LevelContext);
  switch (level) {
    case 1:
      return <h1>{children}</h1>;
    case 2:
      return <h2>{children}</h2>;
    case 3:
      return <h3>{children}</h3>;
    default:
      throw Error('Unknown level: ' + level);
  }
}
export default Heading;

// App.js
import Heading from './Heading.js';
import Section from './Section.js';

const Page = () => {
  return (
    <Section>
      <Heading>Title</Heading>
      <Section>
        <Heading>Heading</Heading>
        <Section>
          <Heading>Sub-heading</Heading>
        </Section>
      </Section>
    </Section>
  );
}
export default Page;
```
