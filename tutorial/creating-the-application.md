---
pos: 2
title: Create App
---

This tutorial is work-in-progress. The goal is create a task management application as [shown here](https://taski.app/). Feedback is welcome!

Found an error? Would you like to suggest an improvement? [Open an issue on GitHub](https://github.com/kreteshq/kretes.dev).

---

Install Kretes globally. Be sure, you have version `0.79` or higher

```
npm i -g kretes
```

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

Since we use TypeScript, let's define the shape of our data. For now, it will be just the `Task` shape that consists of a `name` and the `done` status. In `features/Task/Shape/index.ts` create the following interface

```ts
export interface Task {
  name: string;
  done?: boolean
};
```

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

For convenience, export `TaskCollection` and `TaskInput` from `features/Task/View/index.ts` so that you can import them from the single location:

```ts{2}
export { TaskCollection } from './Collection';
export { TaskInput } from './Input'
```

Put the `TaskInput` component above the `TaskCollection` in `Base/View/index.ts`

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

Refactor the `TaskCollection` component by introducing a sub-component to represent a single task: `TaskElement`.

```tsx
import React from 'react';
import { TaskElement } from './Element';

export const TaskCollection = ({ collection = [] }) => {
  return (
    <ul className="m-0 my-2 p-0 list-none w-full">
      {collection.map(element => <TaskElement {...element} />)}
    </ul>
  );
}
```

Put the single task, i.e. the content of `<li>` tag, in the `TaskElement` component. We want to have a possibility to parametrize the name of each task. Thus, we extract it as the component property (often referred to as *prop*). In this example, our prop is `name` and becomes the input parameter to the `TaskElement` component function.

```tsx{3,13}
import React from 'react';

export const TaskElement = ({ name }) => {
  return (
    <li className="bg-white shadow mb-2">
      <label className="flex justify-start items-center p-4">
        <div className="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2">
          <input type="checkbox" className="opacity-0 absolute" />
          <svg className="fill-current hidden w-4 h-4 text-green-500 pointer-events-none;" viewBox="0 0 20 20">
            <path d="M0 11l2-2 5 5L18 3l2 2L7 18z" />
          </svg>
        </div>
        <div className="ml-2">{name}</div>
      </label>
    </li>
  );
}

```

Optionally, let's make the code in `TaskCollection` more explicit to the compiler by annotating it using the `Task` type we created previously.

```tsx{3,8}
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

Let's display the collection using the in-memory data by fetching it over the wire from an API. Kretes allows to conveniently create REST (and GraphQL) APIs directly in the same application. No more integration hell with connecting the front-end and the back-end.

In this application we will be using a REST API. First, in `features/Task/Controller/browse.ts` create the `browse` handler.

```ts
import { Handler, response } from 'kretes';

const { OK } = response;

const collection = [
  { name: 'Task 1', done: false },
  { name: 'Task 2', done: false },
]

export const browse: Handler = ({ }) => {
  return OK(collection);
}
```

This handler will respond to `GET` requests. It's one of Kretes conventions to help you create web applications faster. In this example the `browse` handler returnes a pre-defined collection of tasks. For now, we statically defined as the in-memory variable `collection`.

Let's start by installng `react-query` so that our frontend can easily communicate with our API.

```
kretes add react-query
```

In `features/Base/View/index.tsx`, define the `request` function for fetching the tasks from the API using browser's built-in `fetch` function (often referred to as the fetch API). Then, pass the `request` function to the `useQuery` hook that comes with `react-query`. This hook will create a client-side cache layer from the data returned by previously implemented `browse` handler.

```tsx{2,6-7,10,12-13,18}
import React from 'react';
import { useQuery } from 'react-query';

import { TaskCollection, TaskInput } from 'Task/View';

const toJSON = _ => _.json()
const request = () => fetch('/task').then(toJSON);

function App() {
  const { data, isLoading, error } = useQuery<any, Error>('tasks', request);

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

```tsx{9-11,13-14,23,26}
import React from 'react';
import { useQuery } from 'react-query';

import { TaskCollection, TaskInput } from 'Task/View';

const toJSON = _ => _.json()
const request = () => fetch('/task').then(toJSON);

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

We can improve the input field by transforming it into a real, fully functional, non-static form. We will be using `react-query-form` to control the form behavior.

First, add the library to your project:

```
kretes add react-hook-form
```

Then, register the input field as managed by `react-hook-form` by using the `register` function that comes from the `useForm` hook. Additionally, you must give the input field a name. In the example, we name it just `name`.

The logic that will be executed once someone clicks the add button is defined in the `onSubmit` function. Here, we wrap our custom logic as an anonymous function passed to `handleSubmit` that comes from the `useForm` hook.

```tsx{2,5-6,9-10}
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

```tsx{10}
import React from 'react';
import { useForm } from "react-hook-form";

export const TaskInput = ({}) => {
  const { register, handleSubmit } = useForm();
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
      <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </form>
  );
}
```

To display possible errors, we need to use the `errors` object that comes from the `useForm` hook. It contains not `null` values for any fields that are registered in our form.


```tsx{5,11}
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

Let's connect our frontend with our backend by using the `react-query` library. First, we need to add it to our project:

```
kretes add react-query
```

Let's start by defining a mutation. We will use the `useMutation` from the `react-query` library. As before we need to define a function that will represent the request that is being sent to our API. Contrary to previous example, this time we need to send a `POST` request, i.e. we want to send what the user entered in the input field to the server-side API. Our data is the `Task` that consists only from the `name`. As we send it as `application/json` we must stringify the payload before passing it as the body of the request.

```tsx{3,5,7-14,19-20}
import React from 'react';
import { useForm } from "react-hook-form";
import { useMutation } from "react-query";

import { Task } from 'Task/Shape';

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

  return (
    <div className="mb-8">
      <form className="flex items-center justify-between relative" onSubmit={onSubmit}>
        <input
          name="name"
          ref={register({ required: true })}
          placeholder="Add new item..."
          type="text"
          className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
        />
        <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
      </form>
      <div>{errors.name && <span>This field is required</span>}</div>
    </div>
  );
}



  // as before
}
```

The problem is that we haven't yet defined a handler in our API to accept this payload. We can cheat a bit by performing a so-called *optimistic update*, i.e. the client state will be updated immediately to reflect the change while the request is being sent to the API. The important detail is that we don't wait for the API to acknowledge the change. Once the data is fetched again, the state will be synchronized automatically. This approach is popular as it improves the user experience and simplifes the state management.

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

Now, we need to start the database engine. There are several ways to do that depending on the platform. Check [the PostgreSQL documentation](https://www.postgresql.org/download/) for details. By default, the PostgreSQL RDBM starts on the port `5432`.

Once the PostgreSQL is started, run `kretes database create` to create a database with the name as defined in `config/default.json`.

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

Once the database setup is finished, we can interact with the data from our handlers. First, let's fetch the data from the database and display it on the frontend. Modify the `features/Task/Controller/browse.ts`

```ts
import { Handler, response, database as db } from 'kretes';

const { OK } = response;

export const browse: Handler = async ({ }) => {
  const collection = await db.from('task')
  return OK(collection);
}
```

Now, we can finally define the API handler for persisting new tasks. Put the following code in `features/Task/Controller/create.ts`:

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

Congratulations! You have now created a fullstack application in TypeScript with React.js as the UI library.
