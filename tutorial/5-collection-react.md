---
pos: 5
title: Collections in React.js
description: >
  Displaying data using collections in React.js
---

# Displaying Collections in React.js

Refactor the `TaskCollection` component by introducing a sub-component to represent a single task: `TaskElement`.

```tsx
import React from 'react';
import { TaskElement } from './Element';

export const TaskCollection = ({ collection = [] }) => {
  return (
    <ul className="m-0 my-2 p-0 list-none w-full">
      {collection.map(element => <TaskElement {...element} />)}
    </ul>
  );
}
```

Put the single task, i.e. the content of `<li>` tag, in the `TaskElement` component. We want to have a possibility to parametrize the name of each task. Thus, we extract it as the component property (often referred to as *prop*). In this example, our prop is `name` and becomes the input parameter to the `TaskElement` component function.

```tsx{3,13}
import React from 'react';

export const TaskElement = ({ name }) => {
  return (
    <li className="bg-white shadow mb-2">
      <label className="flex justify-start items-center p-4">
        <div className="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2">
          <input type="checkbox" className="opacity-0 absolute" />
          <svg className="fill-current hidden w-4 h-4 text-green-500 pointer-events-none;" viewBox="0 0 20 20">
            <path d="M0 11l2-2 5 5L18 3l2 2L7 18z" />
          </svg>
        </div>
        <div className="ml-2">{name}</div>
      </label>
    </li>
  );
}

```

Optionally, let's make the code in `TaskCollection` more explicit to the compiler by annotating it using the `Task` type we created previously.

```tsx{3,8}
import React from 'react';
import { TaskElement } from './Element';
import { Task } from 'Task/Shape';

export const TaskCollection = ({ collection = [] }) => {
  return (
    <ul className="m-0 my-2 p-0 list-none w-full">
      {collection.map((element: Task) => <TaskElement {...element} />)}
    </ul>
  );
}
```
