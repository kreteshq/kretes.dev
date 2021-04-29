---
pos: 5
title: 5. Collections in React.js
description: >
  Displaying data using collections in React.js
images:
- react.svg
prev:
  title: Prototyping with HTML 
  path: tutorial/html-prototype/
next:
  title: Form Handling in React.js 
  path: tutorial/forms-react/
---

# Displaying Collections in React.js

Let's refactor the `TaskCollection` component by introducing another component to represent a single task: `TaskElement`. First, we will generate the scaffold for the `TaskElement` component

```
kretes generate component TaskElement
```

This will gives us the following component at `<project root>/components/TaskElement.tsx`:

```tsx
import React from 'react';

export const TaskElement: React.FC<{}> = () => {
  return (
    <div></div>
  )
}
```

Before we provide the actual body of `TaskElement`, let's use it right away as a building block in `TaskCollection`.

```tsx
import React from 'react';

import { TaskElement } from '@/components';

export const TaskCollection = ({ collection = [] }) => {
  return (
    <ul>
      <TaskElement title="Learn Kretes" />
      <TaskElement title="Build a web application" />
      <TaskElement title="Show your app to the world" />
    </ul>
  );
}
```

We decided to pass the task title as the `title` prop of `TaskElement` to parametrize the title of each task. This decision requires us to adapt the newly created `TaskElement`. We define the title as the component property (often referred to as *prop*). In the following example, our prop is `title` and becomes the input parameter to the `TaskElement` component function.

```tsx
import React from 'react';

export const TaskElement: React.FC<{ title: string }> = ({ title }) => {
  return (
    <li>
      <input type="checkbox" />
      <label>{title}</label>
    </li>
  )
}
```

This is, however, not ideal. We repeat the component three times and the titles for tasks are written directly in the `TaskCollection` component. Let's extract the data outside of that component and focus only on the relation between the collection and its elements.

We will assume that the data is passed into the `TaskCollection` as the `collection` prop. It's an array that represents the collection of our tasks. We can map over it to display as many `TaskElement`s as needed.

```tsx{8}
import React from 'react';

import { TaskElement } from '@/components';

export const TaskCollection = ({ collection = [] }) => {
  return (
    <ul>
      {collection.map(element => <TaskElement title={element.title} />)}
    </ul>
  );
}
```

As the field name in the collection matches the name of the prop in the `TaskElement`, we can simplify the code a bit by destructuring each element directly into the props of the component:

```tsx{8}
import React from 'react';

import { TaskElement } from '@/components';

export const TaskCollection = ({ collection = [] }) => {
  return (
    <ul>
      {collection.map(element => <TaskElement {...element} />)}
    </ul>
  );
}
```

Optionally, let's make the code in `TaskCollection` more explicit to the compiler by annotating it using the `Task` type we created previously.

```tsx{3,9}
import React from 'react';

import { TaskElement } from '@/components';
import { Task } from '@/types';

export const TaskCollection: React.FC<{ collection: Task[] }> = ({ collection = [] }) => {
  return (
    <ul>
      {collection.map((element: Task) => <TaskElement {...element} />)}
    </ul>
  );
}
```

The final result is a 3-element, static list of tasks.
