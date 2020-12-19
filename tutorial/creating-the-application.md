---
pos: 2
title: Create App
---

Found an error? Would you like to suggest an improvement? [Open an issue on GitHub](https://github.com/kreteshq/kretes.dev).

---

Create a new applicaiton using the React.js template

```
kretes new taski.app --template=react
```

In the `features` directory create `Task` and inside it create two other directories `View` and `Controller`:

```
mkdir -p features/Task/{View, Controller}
```

In `features/Task/View` create `Collection.tsx`. This component will be responsible for displaying a list of tasks. We are using Tailwind CSS for styling.

```html
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
```

As we use a custom checkbox with an SVG image for the marking the checked status, we need to make sure to display this image as `block`. Add the following CSS snippet to XXX

```css
input:checked+svg {
  display: block;
}
```

Start the application

```
kretes start
```

Navigate to [localhost:5544](http://localhost:5544) to see the result.

Let's define the shape of our data. For now, it will be just the `Task` shape that consists of a `name` and the `done` status. In `features/Task/Shape/index.ts` create the following interface

```ts
export interface Task {
  name: string;
  done?: boolean
};
```

Let's add a simple form with one input field. In `features/Task/View/Input.tsx` create the following component.

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

For convenience, put `TaskCollection` and `TaskInput` in `features/Task/View/index.ts` so that you can import them from the single location:

```ts
export { TaskCollection } from './Collection';
export { TaskInput } from './Input'
```

Put the `TaskInput` component above the `TaskCollection` in `Base/View/index.ts`

```tsx
<div className="max-w-2xl mx-auto">
  <TaskInput />
  <TaskCollection collection={collection} />
</div>
```

Refactor the `TaskCollection` component by using a component to represent a single task. Put the single task in the `TaskElement` component:

```tsx
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


Let's display the collection using the in-memory data by fetching it over the wire from our API. First, in `features/Task/Controller/browse.ts` create the `browse` handler. It will respond to `GET` requests by returing the pre-defined collection of tasks:

```ts
import { Handler, response } from 'kretes';

const { OK } = response;

const collection = [
  { name: 'Task 11', done: false },
  { name: 'Task 22', done: false },
]

export const browse: Handler = ({ }) => {
  return OK(collection);
}
```

Install `react-query` to easily communicate the API. In `features/Base/View/index.tsx`, define the `request` function for fetching the tasks from the API using browser's built-in `fetch` function (often referred to as the fetch API). Then, pass the `request` function to the `useQuery` hook that comes with `react-query`. This hook will create a client-side cache layer from the data returned by previously implemented `browse` handler.

```tsx
import React from 'react';
import { useQuery } from 'react-query';

import { TaskCollection, TaskInput } from 'Task/View';

const request = () => fetch('/task').then(response => response.json());

interface Children {
  children: React.ReactNode;
}

const Container = ({ children }: Children) =>
  <div className="max-w-2xl mx-auto">{children}</div>

function App() {
  const { data, isLoading, error } = useQuery<any, Error>('tasks', request);

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

We can improve the input field by transforming it into a form. We will be using `react-query-form` to control that form. First, install the library:

```
kretes add react-hook-form
```

Then, register the input field as managed by `react-hook-form` by using the `register` function that comes from the `useForm` hook. Additionally, you must give the input field a name. In the example, we name it `name`.

The logic that will be executed once someone clicks the add button is defined in the `onSubmit` function. Here, we wrap our custom logic as an anonymous function passed to `handleSubmit` that comes from the `useForm` hook.

```tsx
import React from 'react';
import { useForm } from "react-hook-form";

export const TaskInput = ({}) => {
  const { register, handleSubmit } = useForm();
  const onSubmit = handleSubmit(data => console.log(data))

  return (
    <form className="flex items-center justify-between relative mb-8" onSubmit={onSubmit}>
      <input
        name="name"
        ref={register}
        placeholder="Add new item..."
        type="text"
        className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
      />
      <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </form>
  );
}
```

Optionally, we can mark the input field as required. This way, it won't be possible to submit empty tasks. Use the `required` attribute as the `register` parameter.

To display possible errors, we need to use the `errors` object that comes from the `useForm` hook. It contains not `null` values for any fields that are registered in our form.


```tsx
import React from 'react';
import { useForm } from "react-hook-form";

export const TaskInput = ({}) => {
  const { register, handleSubmit, errors } = useForm();
  const onSubmit = handleSubmit(data => console.log(data))

  return (
    <form className="flex items-center justify-between relative mb-8" onSubmit={onSubmit}>
      <input
        name="name"
        ref={register({ required: true })}
        placeholder="Add new item..."
        type="text"
        className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
      />
      {errors.name && <span>This field is required</span>}
      <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </form>
  );
}
```

Let's connect our frontend with our backend by defining a mutation. We will use the `useMutation` from the `react-query` library. As before we need to define a function that will represent the request that is being sent to our API. Contrary to previous example, this time we need to send a `POST` request, i.e. we want to send what the user entered in the input field to the server-side API. Our data is the `Task` that consists only from the `name`. As we send it as `application/json` we must stringify the payload before passing it as the body of the request.

```tsx
const request = (data: Task) =>
  fetch('/task', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data),
  })

export const TaskInput = ({ }) => {
  const { register, handleSubmit, errors } = useForm();

  const mutation = useMutation(request);
  const onSubmit = handleSubmit(data => mutation.mutate(data));

  // as before
}
```

The problem is that we haven't yet defined a handler in our API to accept this payload. We can cheat a bit by performing so-called optimistic update, i.e. the client state will be updated immediately to reflect the change while the request is being sent to the API. The important detail is that we don't wait for the API to acknowledge the change. Once the data is fetched again, the state will be synchronized automatically. This approach is popular as it improves the user experience and simplifes the state management.

```tsx
export const TaskInput = ({ }) => {
  const { register, handleSubmit, errors } = useForm<Task>();

  const queryClient = useQueryClient()
  const mutation = useMutation(request, {
    onMutate: async (task) => {
      await queryClient.cancelQueries('tasks')
      const previousTodos = queryClient.getQueryData<Task[]>('tasks')
      queryClient.setQueryData<Task[]>('tasks', collection => [...(collection as Task[]), task])
      return { previousTodos }
    },
  });
  const onSubmit = handleSubmit(data => mutation.mutate(data));

  // ...
}
```

Before we will be able to implement the handler for accepting new tasks, we need a single location to store our tasks. This way both handlers in our API will have access to the same data. In other words, we need a database.

Let's open the `config/default.json` and define the name of our database under the `db` key.

```json
"db": {
  "database": "acme"
}
```

Now, we need to start the database engine. There are several ways to do that depending on the platform. Check [the PostgreSQL documentation](https://www.postgresql.org/download/) for details. By default, the PostgreSQL RDBM starts on the port `5432`. Run `kretes database create` to create a database as defined in `config/default.json`.

Now we need to define the shape of data in our database. As we only have tasks at the moment, there will be just one table. Put the following content in `db/setup.sql`

```sql
create table task (
  id serial primary key,
  name text,
  done boolean default false
);

insert into task (name) values
  ('Learn React.js'),
  ('Create your first API using TypeScript'),
  ('Master the Kretes framework');
```

Then, run the setup method

```
kretes database setup
```

Once the database setup is finished, we can interact with the data from our handlers. First, let's fetch the data from the database and display it on the frontend.


```ts
import { Handler, response, database as db } from 'kretes';

const { OK } = response;

export const browse: Handler = async ({ }) => {
  const collection = await db.from('task')
  return OK(collection);
}
```

Now, we can finally define the API handler for persisting new tasks

```ts
import { Handler, response, database as db } from 'kretes';

const { Created } = response;

export const create: Handler = async ({ params }) => {
  const { name } = params;

  await db.from('task').insert({ name });

  return Created();
}
```

Optionally, let's clear the input field once a new task is added.

```ts
const { register, handleSubmit, errors, reset } = useForm<Task>();

const onSubmit = handleSubmit(data => {
  mutation.mutate(data)
  reset();
});
```

