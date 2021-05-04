---
pos: 15
title: 15. Using Tailwind CSS
description: >
  Let's use Tailwind CSS to design and build the UI prototype.
images:
- tailwindcss.svg
prev:
  title: Docker Image 
  path: tutorial/docker/
next:
  title: Summary 
  path: tutorial/finishing-up/
---

# Using Tailwind CSS in Kretes

CSS in Kretes is located in the `stylesheets` directory. By convention the `main.css` is the entry point. You can use it as the only file containing your CSS or you can split it into several files and import using the `@import` directive.

Let's start by creating the configuration files for Tailwind. We need `tailwind.config.js` and `postcss.config.js`. Both must be placed in the `config/` directory.

`tailwind.config.js`:
```js
module.exports = {
  theme: {},
  variants: {},
  plugins: [],
}
```

`postcss.config.js`:
```js
const tailwindcss = require('tailwindcss');
const autoprefixer = require('autoprefixer');

module.exports = {
  plugins: [
    tailwindcss('./config/tailwind.config.js'),
    autoprefixer,
  ]
};
```

Now, let's add `tailwindcss` and `autoprefixer` to the project as development dependencies:

```
kretes add -D tailwindcss autoprefixer
```

In `site/index.html` replace the `<link>` tag pointing to CDN location of the MVP.css:

```html
<link rel="stylesheet" href="https://unpkg.com/mvp.css@1.6.3/mvp.css">
```


with the CSS link pointing to our local `main.css` file:

```html
<link rel="stylesheet" href="/main.css">
```

In `stylesheets/main.css`, put the Tailwind directives:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
---

Let's use Tailwind classes in React.js components starting from the top with `App`

```tsx
export const App = () => {
  return (
    <div className="max-w-2xl mx-auto">
      <Hero />
      <main>
        <TaskInput />
        <TaskCollection />
      </main>
    </>
  );
}
```

Next, let's adapt `TaskInput`:

```tsx
import React from 'react';

export const TaskInput: React.FC<{}> = ({}) => {
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

`TaskCollection` needs just a small change to the list container (`<ul>`):

```tsx
import React from 'react';
import { TaskElement } from '@/components';
import { Task } from '@/types';

export const TaskCollection: React.FC<{ collection: Task[] }> = ({ collection = [] }) => {
  return (
    <ul className="m-0 my-2 p-0 list-none w-full">
      {collection.map((element: Task) => <TaskElement {...element} />)}
    </ul>
  );
}
```

In `TaskElement` there are some major changes:

```tsx
import React from 'react';

export const TaskElement: React.FC<{ title: string }> = ({ title }) => {
  return (
    <li className="bg-white shadow mb-2">
      <label className="flex justify-start items-center p-4">
        <div className="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2">
          <input type="checkbox" className="opacity-0 absolute" />
          <svg className="fill-current hidden w-4 h-4 text-green-500 pointer-events-none;" viewBox="0 0 20 20">
            <path d="M0 11l2-2 5 5L18 3l2 2L7 18z" />
          </svg>
        </div>
        <div className="ml-2">{title}</div>
      </label>
    </li>
  )
}
```

As we are using a custom shape for the checkbox from SVG, there is an additional CSS change. Put the following snippet at the bottom of `stylesheets/main.css`

```css
input:checked+svg {
  display: block;
}
```

Finally, a bunch of changes to the `Hero` component

```tsx
import React from 'react';

export const Hero: React.FC<{}> = ({}) => {
  return (
    <header className="my-8">
      <h1 className="text-4xl tracking-tight font-extrabold text-gray-900">
        <span>Taski</span><span className="text-blue-400">.app</span>
      </h1>
      <h3 className="text-xl tracking-wide text-gray-700">A task manager with <u>Kretes</u></h3>
    </header>
  );
}
```

## Result

![Taski.app](/images/tutorial/tutorial-15.png#center)


