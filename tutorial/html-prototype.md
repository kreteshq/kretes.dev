---
pos: 4
title: 4. Prototyping with HTML
description: >
  Using HTML to build the UI prototype.
prev:
  title: Using TypeScript 
  path: tutorial/typescript-shape/
next:
  title: Collections in React.js
  path: tutorial/collection-react/
---

# Prototyping with HTML

Let's generate a React.js component for displaying a collection of tasks.

```
kretes generate component TaskCollection
```

<Notice>
`kretes` can be abbreviated to just `ks`, `generate` to just `g` and `component` to just `comp` i.e. the command `ks g comp TaskCollection` will also work
</Notice>

This command will create a file in `<project root>/components/`. Open the `TaskCollection.tsx` and replace the following content:

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
    <ul>
      <li>
        <input type="checkbox" />
        <label>Learn Kretes</label>
      </li>
      <li>
        <input type="checkbox" />
        <label>Build a web application</label>
      </li>
      <li>
        <input type="checkbox" />
        <label>Show your app to the world</label>
      </li>
    </ul>
  )
}
```

It is a list (`<ul>`) of three elements (`<li>`) where each represent a task containing a checkbox and a label.

Go to `<project root>/components/App.tsx` and import newly created `TaskCollection` component as shown below:

```tsx{8}
import React from 'react';

import { TaskCollection } from '@/components';

const App: React.FC<{}> = () => {
  return (
    <main>
      <TaskCollection />
    </main>
  );
}

export { App };
```

<Notice>
`@/[directory name]` is an alias for conveniently refer to files in the root of your project (e.g. `components/`) without using relative paths, such as `../` or `../..`.
</Notice>

---

A task manager also needs a way to enter tasks. We will use another component for that, let's call it `TaskInput`.

```
kretes generate component TaskInput
```

Our component needs to combine an input field for entering the actual task with a button for submitting the new task. This will constitute a form. This form is static because nothing happens when the button is clicked as we are only concerned with how it's displayed in the UI at this point

```tsx
import React from 'react';

export const TaskInput: React.FC<{}> = ({}) => {
  return (
    <form>
      <input
        placeholder="Add new item..."
        type="text"
      />
      <button type="submit">Add</button>
    </form>
  );
}
```

Let's add it to `App` right above the `TaskCollection` component:

```tsx{8}
import React from 'react';

import { TaskCollection, TaskInput } from '@/components';

const App: React.FC<{}> = () => {
  return (
    <main>
      <TaskInput />
      <TaskCollection />
    </main>
  );
}

export { App };
```

Optionally, we can add yet another component for the header section, let's call it `Hero`:

```
kretes generate component Hero
```

As before, this will create the `Hero.tsx` file in `<project root>/components/`. Let's open it and change its content as follows:

```tsx
import React from 'react';

export const Hero: React.FC<{}> = () => {
  return (
    <header>
      <h1>Taski.app</h1>
      <h3>A task manager with <u>Kretes</u></h3>
    </header>
  )
}
```

Let's add it to `App` as before, right above the `<main>` tag. As React.js requires one root element per component, we need to wrap the content into a Fragment. The `<> .. </>` is a convenience shortcut for that.

```tsx{8}
import React from 'react';

import { TaskCollection, TaskInput, Hero } from '@/components';

const App: React.FC<{}> = () => {
  return (
    <>
      <Hero />
      <main>
        <TaskInput />
        <TaskCollection />
      </main>
    </>
  );
}

export { App };
```

---


Start the application

```
kretes start
```

Navigate to [localhost:5544](http://localhost:5544) to see the result. There should be a single task that we statically defined in the `TaskCollection` component.

## Result

By default Kretes includes a [MVP.css](https://andybrewer.github.io/mvp/). It's a minimalist stylesheet for HTML elements. You don't need to use CSS to have a coherent look'n'feel for the most HTML elements.

![Taski.app](/images/tutorial/tutorial-4.png#center)

The MVP.css is defined as a `<link>` reference in the `<project root>/site/index.html`. Later on, we will replace it with Tailwind CSS to show a CSS integration in Kretes. You can, however, use any CSS approach you like.
