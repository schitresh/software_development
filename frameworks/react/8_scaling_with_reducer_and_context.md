## Scaling with Reducer and Context
- It is common to use a reducer together with context to manage complex state
  - And pass it down to distant components without too much hassle
  - This avoids passing state & event handlers as props to each & every component
- If you pass a different value on the next render
  - React will update all the components reading it below

```js
// TasksContext.js
import { createContext, useContext, useReducer } from 'react';

const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

// Functions like useTasks and useTasksDispatch are called Custom Hooks
export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, { id: action.id, text: action.text, done: false }];
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

```js
// App.js
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';
import { TasksProvider } from './TasksContext.js';

export default function TaskApp() {
  return (
    <TasksProvider>
      <AddTask/>
      <TaskList/>
    </TasksProvider>
  );
}
```

```js
// AddTask.js
import { useState, useContext } from 'react';
import { TasksDispatchContext } from './TasksContext.js';

export default function AddTask() {
  const [text, setText] = useState('');
  const dispatch = useContext(TasksDispatchContext);

  const handleTaskAdd = () => {
    setText('');
    dispatch({ type: 'added', id: generateNextId(), text: text });
  }

  return (
    <>
      <input
        placeholder='Add task'
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
      <button onClick={handleTaskAdd}>
        Add
      </button>
    </>
  );
}
```

```js
// TaskList.js
import { useState, useContext } from 'react';
import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskList() {
  const tasks = useContext(TasksContext);
  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>
          <Task task={task}/>
        </li>
      ))}
    </ul>
  );
}

function Task({ task }) {
  const [isEditing, setIsEditing] = useState(false);
  const dispatch = useContext(TasksDispatchContext);

  const handleTaskEdit = (e) => {
    dispatch({ type: 'changed', task: { ...task, text: e.target.value } });
  }
  const handleTaskDone = (e) => {
    dispatch({ type: 'changed', task: { ...task, done: e.target.checked } });
  }
  const handleTaskDelete = () => {
    dispatch({ type: 'deleted', id: task.id });
  }

  const renderTaskEdit = () => (
    <>
      <input value={task.text} onChange={handleTaskEdit}/>
      <button onClick={() => setIsEditing(false)}>
        Save
      </button>
    </>
  );
  const renderTaskText = () => (
    <>
      {task.text}
      <button onClick={() => setIsEditing(true)}>
        Edit
      </button>
    </>
  );

  return (
    <label>
      <input
        type='checkbox'
        checked={task.done}
        onChange={handleTaskDone}
      />
      {isEditable ? renderTaskEdit() : renderTaskText()}
      <button onClick={handleTaskDelete}>
        Delete
      </button>
    </label>
  );
}
```
