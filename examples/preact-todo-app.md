---
pos: 2
title: ToDo App with Preact
---
# ToDo App with Preact

## Prerequisites

Follow the [Setting up Preact]() tutorial to set up the proper base for this ToDo application.

## Custom CSS

You can use a custom CSS for a nicer visual effect. Add the following CSS link to the `<head>` section in `config/client/index.html`:

```html
<link rel="stylesheet" href="http://fip.zaiste.net/main.css">
```

## Create `App` Component

In `features/Base/View/index.tsx` replace the content with the following code:

```jsx
import { h } from 'preact';
import { useState, useEffect } from 'preact/hooks';

interface Task {
  name: string;
  done: boolean
};

function App() {
  const [newTask, setNewTask] = useState('');
  const [tasks, setTasks] = useState([] as Task[]);

  const addTodo = () => {
    setTasks([...tasks, { name: newTask, done: false }]);
    setNewTask('');
  }

  const onInput = (event: Event) => {
    const { value } = event.target as HTMLInputElement;
    setNewTask(value)
  }

  const onKeyUp = (event: KeyboardEvent) => {
    const { key } = event;
    if (key === 'Enter') {
      addTodo();
    }
  }

  return (
    <div class="task-container">
      <div class="task-input">
        <input
          placeholder="Add new item..."
          type="text"
          class="task-new"
          value={newTask}
          onInput={onInput}
          onKeyUp={onKeyUp}
        />
        <button class="task-add" onClick={addTodo}>Add</button>
      </div>
      <ul class="task-list">
        {tasks.map((task, index) => (
          <li key={index} class="task-item" v-for="task in state.tasks">
            <label class="task-item-container">
              <div class="task-checkbox">
                <input type="checkbox" class="opacity-0 absolute" value={task.done} />
                <svg class="task-checkbox-icon" viewBox="0 0 20 20">
                  <path d="M0 11l2-2 5 5L18 3l2 2L7 18z" />
                </svg>
              </div>
              <div class="ml-2">{task.name}</div>
            </label>
          </li>
        ))}
      </ul>
    </div>
  );
}

export { App };
```

## REST Endpoint

In `features/Task/Controller` create `browse.ts`. The naming is important. The `browse` name corresponds to the HTTP `GET` request that is supposed to respond with a collection of a given resource. In this case, it's a collection of tasks.

We create a handler function that describes the action that will happen once such request is received. In Kretes, handlers are functions that take HTTP requests as input and return HTTP responses as output. The `OK` convenience wrapper corresponds to the `200 OK` response. We define the list of tasks in memory for now, as the `tasks` variable.

```tsx
import { Handler, response } from 'kretes';

const { OK } = response;

const tasks = [
  { name: 'Use TypeScript 4.x', done: false },
  { name: 'Learn Kretes', done: false },
  { name: 'Enjoy programming', done: false },
]

export const browse: Handler = ({ params }) => {
  return OK(tasks);
}
```

## Data Fetching

Connect the front-end with the REST endpoint using the `useEffect` hook and the built-in `fetch` browser API.

```tsx
const fetchData = async () => {
  const response = await fetch('/task');
  const result = await response.json();
  setTasks(result);
};

useEffect(() => fetchData(), [])
```

## Result

![Preact ToDo with Kretes](https://user-images.githubusercontent.com/200613/90222429-55485900-de0c-11ea-8a52-49fbbcf6cd3c.gif)
