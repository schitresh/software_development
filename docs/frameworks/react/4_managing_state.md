## Managing State
- Instead of manipulating individual pieces of UI directly (imperative UI)
  - You describe different states that the component can be in
  - And switch between them in response to the user input
- This is declarative UI which is better than imperative UI because
  - State can control multiple behaviors
  - You don't have to write exact instruction for each event

### Identify the component's different visual states
- For example, a form can have these states
  - Empty: Form has a disabled 'Submit' button
  - Typing: Form has an enabled 'Submit' button
  - Submitting: Form is completely disabled, spinner is shown
  - Success: 'Thank you' message is shown instead of a form
  - Error: Same as Typing state, but with an extra error message

### Determine what triggers the state changes
- There can be two kinds of trigger
  - Human inputs, like clicking a button, typing in a field
  - Computer inputs, like receiving network response, timeout, image loaded
- For example, a form can have these triggers
  - Changing the text input: Should switch from Empty to Typing state or back
    - Depending on whether the text box is empty or not
  - Clicking the Submit button: Should switch to Submitting state
  - Successful network response: Should switch to Success state
  - Failed network response: Should switch to Error state with the error message

### Represent the state in memory using useState
- Start with the state that absolutely must be there
- Each piece of state is a moving piece and you want as few moving pieces as possible
- More complexity leads to more bugs

### Remove any non-essential state variables
- Avoid duplication in the state content so you’re only tracking what is essential
  - It is difficult to keep duplicate items in sync
- Prevent the cases where the state in memory doesn’t represent any valid UI
  - E.g. Show an error message and disable the input at the same time
    - How will the user correct the error?
- Prevent paradoxes or contradictions
  - E.g. isTyping, isSubmitting, and isSuccess can't all be true at the same time
    - So they can be combined into a single state called 'status'
- Check if the same information is available in another state
  - E.g. isError is not needed because you can check error !== null instead
- Avoid deeply nested state
  - It is inconvenient to update a single item in large amount of data

### Connect event handlers to set the state
- Expressing all interactions as state changes
  - Lets you later introduce new visual states without breaking existing ones
- It also lets you change what should be displayed in each state
  - Without changing the logic of the interaction itself

```js
import { useState } from 'react';

const Form = () => {
  const [answer, setAnswer] = useState('');
  const [error, setError] = useState(null);
  const [status, setStatus] = useState('typing');

  if (status === 'success') return <h1>Submitted</h1>

  const handleSubmit = async (e) => {
    e.preventDefault();
    setStatus('submitting');
    try {
      await submitForm(answer);
      setStatus('success');
    } catch (err) {
      setStatus('typing');
      setError(err);
    }
  }

  const handleTextChange = (e) => { setAnswer(e.target.value); }

  return (
    <form onSubmit={handleSubmit}>
      <textarea
        value={answer}
        onChange={handleTextChange}
        disabled={status === 'submitting'}
      />
      <br/>
      <button disabled={answer.length === 0 || status === 'submitting'}>
        Submit
      </button>
      {error !== null && <p className="Error">{error.message}</p>}
    </form>
  );
}
export default Form;

function submitForm(answer) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (answer.toLowerCase() !== 'answer') {
        reject(new Error('Good guess but a wrong answer. Try again!'));
      } else {
        resolve();
      }
    }, 1500);
  });
}
```

## Sharing State
- Sometimes you want the state of two components to always change together
- To do that, lift state up
  - Remove state from both of them and move it to their closest common parent
  - Then pass it down to them via props

```js
import { useState } from 'react';

const Accordion = () => {
  const [activeIndex, setActiveIndex] = useState(0);
  return (
    <>
      <Panel
        title='Panel 1'
        isActive={activeIndex === 0}
        onShow={() => setActiveIndex(0)}
      >
        Description 1
      </Panel>
      <Panel
        title='Panel 2'
        isActive={activeIndex === 1}
        onShow={() => setActiveIndex(1)}
      >
        Description 1
      </Panel>
    </>
  );
}
export default Accordion;

const Panel({ title, children, isActive, onShow }) => (
  <section className='panel'>
    <h3>{title}</h3>
    {isActive ? <p>{children}</p> : <button onClick={onShow}>Show</button>}
  </section>
);
```
