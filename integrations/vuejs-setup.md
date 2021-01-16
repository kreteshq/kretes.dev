---
pos: 2
title: Vue.js 3
logo: vue.svg
---
# Setting up Vue.js 3

[Vue.js](https://vuejs.org/) is a progressive framework for building user interfaces. Vue.js 3 introduces Composition API that allows for a function-based way of writing components. This approach is inspired by React Hooks and allows to encapsulate logic into "composition functions" and reuse that logic across components. Vue.js 3 combined with Kretes allows to quickly build **full-stack** TypeScript applications.

## Install Vue.js 3

```
kretes add vue@3.0.0-rc.5
```

## Init Script

In `config/client/index.html`, just before the closing `</body>` tag, add the init script pointing to `config/client/index.ts`:

```html
  ...
  <script type="module" src="/config/client/index.ts"></script>
</body>
```

Inside `config/client/index.ts` put the following initialization script:

```ts
import { createApp } from 'vue';
import App from 'Base/View/App.vue';

createApp(App).mount('#app');
```

## Create `App` Component

In `Base/View/App.vue`, let's use `<suspense>` to prepare our application for asynchrounous communication with the back-end.

```html
<template>
  <suspense>
    <template #default>
      <Welcome />
    </template>
    <template #fallback>
      <div>Loading ...</div>
    </template>
  </suspense>
</template>
<script>
import Welcome from 'Base/View/Welcome.vue';

export default {
  components: { Welcome },
  setup() {},
};
</script>
```

In `Base/View/Welcome.vue`, let's create a simple component (`<Welcome>`) that says *Hello* to a fixed name.

```html
<template>
  <div class="max-w-2xl mx-auto">
    <div class="p-4 bg-white shadow">
      Hello, <span class="font-semibold">{{name}}</span>
    </div>
  </div>
</template>
<script>
export default {
  setup() {
    const name = 'Zaiste';

    return { name }
  },
};
</script>
```


## REST Endpoint

Kretes is all about building full-stack applications in TypeScript. Let's create a REST endpoint that returns a name and let's connect it with our `Welcome` component to dynamically display it.

In `features/Base/Controller` create `browse.ts`. The naming is important. The `browse` name corresponds to the HTTP `GET` request that is supposed to respond with a collection of a given resource. In this case for simplicity reasons, we will return a collection of one element - a hash with the `name` key and a random name as its value.

```ts
import { Handler, response } from 'kretes';

const { OK } = response;

const names = [ 'Rick Deckard', 'Harry Bryant', 'Roy Batty', 'Dr. Eldon Tyrell', 'Hannibal Chew',
  'Niander Wallace', 'Taffey Lewis', 'Dave Holden', 'Ana Stelline', 'Leon Kowalski'];

export const browse: Handler = ({ params }) => {
  const name = names[Math.floor(Math.random() * names.length)];

  return OK([{ name }]);
}
```

We create a handler function that describes the action that will happen once such request is received. In Kretes, handlers are functions that take HTTP requests as input and return HTTP responses as output. The `OK` convenience wrapper corresponds to the `200 OK` response. We define the list of names in memory for convenience, as the `names` variable.

## Data Fetching

We can now connect the front-end with the REST endpoint using the built-in `fetch` browser API. In the `Welcome` component, change the `setup()` function with the following piece of code:

```tsx
export default {
  async setup() {
    const response = await fetch('/base');
    const [{ name }] = await response.json();

    return { name }
  },
};
```

## Result

🎉  Congrats! You built a simple, but still full-stack TypeScript application using Vue.js 3 and Node.js. Did you notice there's no Webpack or Rollup setup needed? It works out-of-the-box with **zero configuration**. That's cool, isn't?

Here's the [source code](https://github.com/kreteshq/vuejs3-kretes-setup-example) for this application

![Vue.js 3 with Kretes](https://user-images.githubusercontent.com/200613/90030019-d80dce80-dcbb-11ea-9f7c-928050f8943c.gif)

