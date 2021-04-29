---
pos: 8
title: 8. Fetching from API in React.js using React Query
description: >
  Using React Query to integrate with a remote REST API endpoint
images:
- react-query.svg
prev:
  title: RESTful APIs 
  path: tutorial/restful-api/
next:
  title: Sending Data to a RESTful API with React.js
  path: tutorial/reactjs-rest-api-sending/
---

# Fetching Data from a RESTful API with React.js

Let's now implement in React.js the mechanism of fetching the data from server. Since the server data is exposed as a RESTful API, we need to send a `GET` request from a React.js component to a specific endpoint. In the previous step, we implemented a handler returning the collection of tasks and exposed it under the `/_api/task` endpoint.

At this point we could've just used the `fetch` function (that's built-in into the browser) to make the request from React.js. This is, however, a bit more complicated as the data we operate on is never static. In our application, it seems static so far, because we defined the task collection as a constant. Eventually though, this collection will be changing during the application life-time once we implement the feature for adding new tasks.

Fetching data from the server and synchronizing it with the application state on the client (i.e. in the browser) is not a trival job. There is a bunch of important details for any client application to take care of. Luckly, there is `react-query`, a React.js library that solves the client-server integration pains and more. Thanks to it, our React.js components can easily and reliabily communicate with remote data locations.

Similarly to `react-hook-form`, `react-query` is also included in the Kretes React.js template, no need to install it. We will add the data fetching mechanism in our top level React.js component: `App`. In `features/Base/View/index.tsx`, let's add the `request` function that defines the logic for fetching the tasks from our API using browser's built-in `fetch` function (referred to as the fetch API). Then, pass the `request` function to the `useQuery` hook that comes with `react-query`. This hook will create a client-side cache layer from the data returned by previously implemented `browse` handler. From now, the data exchange between the client and server part of our application is handled by React Query.

```tsx{2,7-8,11,13-14,19}
import React from 'react';
import { useQuery } from 'react-query';

import { TaskCollection, TaskInput } from '@/components';
import { Task } from '@/types';

const toJSON = (response: Response) => response.json()
const request = () => fetch('/_api/task').then(toJSON);

const App: React.FC<{}> = () => {
  const { data, isLoading, error } = useQuery<Task[], Error>('tasks', request);

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return (
    <div className="max-w-2xl mx-auto">
      <TaskInput />
      <TaskCollection collection={data} />
    </div>
  );
}

export { App };
```

Before we move to another topic, let's finish up this section by refactoring a bit our `App` component. We can extract the top-level HTML snippet to a separate component. This will be a plain container for the application layout. Such code organization will allow us easily to reuse in the future the same layout in different pages across the entire application.

```tsx{10-11,20,23}
import React from 'react';
import { useQuery } from 'react-query';

import { TaskCollection, TaskInput } from '@/components';
import { Task } from '@/types';

const toJSON = (response: Response) => response.json()
const request = () => fetch('/_api/task').then(toJSON);

const Container: React.FC<{}> = ({ children }) =>
  <div className="max-w-2xl mx-auto">{children}</div>

const App: React.FC<{}> = () => {
  const { data, isLoading, error } = useQuery<Task[], Error>('tasks', request);

  if (isLoading) return <div>Loading...</div>
  if (error) return <div>Error: {error.message}</div>

  return (
    <Container>
      <TaskInput />
      <TaskCollection collection={data} />
    </Container>
  );
}

export { App };
```
