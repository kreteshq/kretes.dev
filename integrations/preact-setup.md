---
pos: 3
title: Preact
logo: preact.svg
---

# Setting up Preact

[Preact](https://preactjs.com/) is a fast, 3kB alternative to React.js with the same API. Preact combined with Kretes allows to quickly build **full-stack** TypeScript applications.

## Install Preact

```
kretes add preact
```

## TypeScript Configuration

In `config/client/tsconfig.json` add the following options under the `compilerOptions` so that there won't be any errors for the JSX syntax:

```json
"jsx": "react",
"jsxFactory": "h",
"jsxFragmentFactory": "Fragment"
```

## Init Script

In `config/client/index.html`, just before the closing `</body>` tag, add the init script pointing to `config/client/index.tsx`:

```html
  ...
  <script type="module" src="/config/client/index.tsx"></script>
</body>
```

In `config/client` rename `index.ts` to `index.tsx` if you plan to use the JSX syntax.

Inside `config/client/index.tsx` put the following initialization script:

```jsx
import { h, render } from 'preact';
import { App } from 'Base/View';

render(<App />, document.getElementById('app')!);
```

## Create `App` Component

Let's create a simple component that says *Hello* to a fixed name (set in the application state).

```jsx
import { h } from 'preact';
import { useState } from 'preact/hooks';

function App() {
  const [name, setName] = useState('Zaiste');

  return (
    <div class="max-w-2xl mx-auto">
      <div class="p-4 bg-white shadow">
        Hello, <span class="font-semibold">{name}</span>
      </div>
    </div>
  );
}

export { App };
```


## REST Endpoint

Kretes is all about building full-stack applications in TypeScript. Let's create a REST endpoint that returns a name and let's connect it with our `App` component to dynamically display it.

In `features/Base/Controller` create `browse.ts`. The naming is important. The `browse` name corresponds to the HTTP `GET` request that is supposed to respond with a collection of a given resource. In this case for simplicity reasons, we will return a collection of one element - a hash with the `name` key and a random name as its value.

```tsx
import { Handler, response } from 'kretes';

const { OK } = response;

const names = [ 'Rick Deckard', 'Harry Bryant', 'Roy Batty', 'Dr. Eldon Tyrell', 'Hannibal Chew',
  'Niander Wallace', 'Taffey Lewis', 'Dave Holden', 'Ana Stelline', 'Leon Kowalski'];

export const browse: Handler = ({ params }) => {
  const nameAtRandom = names[Math.floor(Math.random() * names.length)];

  return OK({ name: nameAtRandom });
}

```

We create a handler function that describes the action that will happen once such request is received. In Kretes, handlers are functions that take HTTP requests as input and return HTTP responses as output. The `OK` convenience wrapper corresponds to the `200 OK` response. We define the list of names in memory for convenience, as the `names` variable.

## Data Fetching

We can now connect the front-end with the REST endpoint using the `useEffect` hook and the built-in `fetch` browser API. In the `App` component function just after `useState` add the following piece of code

```tsx
function App() {
  const [name, setName] = useState('');

  const fetchData = async () => {
    const response = await fetch('/base');
    const { name } = await response.json();
    setName(name);
  };

  useEffect(() => {
    fetchData();
  }, [])

  ...
}
```

## Result

🎉 Congrats! You built a simple, but still full-stack TypeScript application using Preact and Node.js. Did you notice there's no Webpack or Rollup setup needed? It works out-of-the-box with **zero configuration**. That's cool, isn't?

Here's the [source code](https://github.com/kreteshq/preact-kretes-example) for this application

![Preact with Kretes](https://user-images.githubusercontent.com/200613/90030019-d80dce80-dcbb-11ea-9f7c-928050f8943c.gif)

