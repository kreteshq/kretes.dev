---
pos: 4
title: 4. Lists with Tailwind CSS
description: >
  Using Tailwind CSS to build the UI prototype.
---

# Prototyping with Tailwind CSS

Kretes comes with Tailwind CSS integration out of the box.

## Steps

Let's generate a React.js component for displaying a collection of tasks.

```
kretes generate view Task TaskCollection
```

This command will create a file in `<project root>/features/Task/View`. Open the `TaskCollection.tsx` and replace the following content:

```tsx
import React from 'react';

export const TaskCollection: React.FC<{}> = () => {
  return (
    <div></div>
  )
}
```

with this:

```tsx
import React from 'react';

export const TaskCollection: React.FC<{}> = () => {
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

Go to `<project root>/features/Base/View/index.tsx` and import newly created `TaskCollection` component as shown below:

```tsx{8}
import React from 'react';

import { TaskCollection } from '@/Task/View';

const App: React.FC<{}> = () => {
  return (
    <div className="max-w-2xl mx-auto">
      <TaskCollection />
    </div>
  );
}

export { App };
```

`@/<something>` is an alias for conveniently refer to features and their files without using relative paths, such as `../` or `../..`.

Start the application

```
kretes start
```

Navigate to [localhost:5544](http://localhost:5544) to see the result. There should be a single task that we statically defined in the `TaskCollection` component.

## Result
