---
pos: 4
title: Lists with Tailwind CSS
description: >
  Using Tailwind CSS to build the UI prototype.
---

# Prototyping with Tailwind CSS

## Prerequisites

None. Kretes comes with Tailwind CSS integration out of the box.

## Steps

In the `features` directory create `Task` and inside it create another directory: `View`

```
mkdir -p features/Task/View
```

In `features/Task/View` create `Collection.tsx`. This component will be responsible for displaying a list of tasks.

```tsx
import React from 'react';

export const TaskCollection = ({}) => {
  return (
    <ul className="m-0 my-2 p-0 list-none w-full">
      <li className="bg-white shadow mb-2">
        <label className="flex justify-start items-center p-4">
          <div className="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2">
            <input type="checkbox" className="opacity-0 absolute" />
            <svg className="fill-current hidden w-4 h-4 text-green-500 pointer-events-none;" viewBox="0 0 20 20">
              <path d="M0 11l2-2 5 5L18 3l2 2L7 18z" />
            </svg>
          </div>
          <div className="ml-2">A task name</div>
        </label>
      </li>
    </ul>
  )
}
```

As we use a custom checkbox with an SVG image for the marking the checked status, we need to make sure to display this image as `block`. Add the following CSS snippet to `<head>` section of `index.html`.

```css
<style>
  input:checked+svg {
    display: block;
  }
</style>
```

For convenience, export `TaskCollection` from `features/Task/View/index.ts` so that later on you can import all directory components from the single location:

```ts
export { TaskCollection } from './Collection';
```

Import `TaskCollection` in `features/Base/View/index.tsx`

```tsx{8}
import React from 'react';

import { TaskCollection } from 'Task/View';

function App() {
  return (
    <div className="max-w-2xl mx-auto">
      <TaskCollection />
    </div>
  );
}

export { App };
```

Start the application

```
kretes start
```

Navigate to [localhost:5544](http://localhost:5544) to see the result. There should be a single task that we statically defined in the `TaskCollection` component.

## Result
