## Reducer
- Components with many state updates spread across many event handlers can get overwhelming
- Reducer is used to consolidate state update logic outside the component for such cases
- It helps to cut down the code if many handlers modify state in a similar way

## Migrating from State to Reducer
- Move from setting state to dispatching actions
  - Instead of telling react 'what to do' by setting state
  - Specify 'what the user just did' by dispatching 'actions' from event handlers
  - The state update logic will live elsewhere
- Write a reducer function
  - Where state logic is written
  - Takes two arguments, the current state and the action object
  - Returns the next state
  - It's convention to use switch statements for different values for readability
- Use the reducer from your component
  - Replace useState to useReducer
  - `const [tasks, setTasks] = useState(initialTasks);`
  - `const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);`

```js
import { useReducer } from 'react';

const TaskApp = () => {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  const handleAddTask = (text) => {
    dispatch({ type: 'added', id: generateNextId(), text: text });
  }
  const handleChangeTask = (task) => {
    dispatch({ type: 'changed', task: task });
  }
  const handleDeleteTask = (taskId) => {
    dispatch({ type: 'deleted', id: taskId });
  }

  return (
    <>
      <AddTask onAddTask={handleAddTask}/>
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}
export default TaskApp;

const tasksReducer = (tasks, action) => {
  // It's recommended wrapping each case block into curly braces
  // so that variables within different cases don't clash with each other
  switch (action.type) {
    case 'added': {
      return [ ...tasks, { id: action.id, text: action.text, done: false } ];
    }
    case 'changed': {
      return tasks.map((t) => t.id === action.task.id ? action.task : t);
    }
    case 'deleted': {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}
```
