---
pos: 6
title: Forms with Tailwind CSS
description: >
  Let's use Tailwind CSS to design and build the UI prototype.
---

# Creating Forms in HTML with Tailwind CSS

Let's build a non-functional, static form with one input field. It's static because nothing happens when the *Add* button is clicked. It's non-functional as we are only concerned with how it's displayed in the UI at this point.

In `features/Task/View/Input.tsx` create the following component.

```tsx
import React from 'react';

export const TaskInput = ({}) => {
  return (
    <div className="flex items-center justify-between relative mb-8">
      <input
        placeholder="Add new item..."
        type="text"
        className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
      />
      <button className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </div>
  );
}
```

For convenience, add `TaskInput` to `features/Task/View/index.ts` so that you can import all those components from the single location:

```ts{2}
export { TaskCollection } from './Collection';
export { TaskInput } from './Input'
```

Then, put the `TaskInput` component above the `TaskCollection` in `Base/View/index.ts`

```tsx{3,8}
import React from 'react';

import { TaskCollection, TaskInput } from 'Task/View';

function App() {
  return (
    <div className="max-w-2xl mx-auto">
      <TaskInput />
      <TaskCollection />
    </div>
  );
}

export { App };
```
