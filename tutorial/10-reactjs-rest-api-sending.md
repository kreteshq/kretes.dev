---
pos: 10
title: 10. Sending to API in React.js
description: >
  Let's use Tailwind CSS to design and build the UI prototype.
---

# Sending Data to a RESTful API with React.js

In the previous step, we managed to fetch data from the server and display the collection of tasks on the client in our React.js application. Let's do the opposite: when users add a new task via the form, we need to send it to the server and eventually persist in the database. We will be using the same endpoint in our API, i.e. `/_api/task`. The only difference is the type of invoked action, i.e. instead of doing a `GET` request, we will send a `POST` request along with the payload containing the new task to add.

As for data fetching, we will use the react-query library to also send data out. Since the intention is to change the server data (we want to append a new task to the collection), we need to define a *mutation* via the `useMutation` hook. Similarly to `useQuery`, this hook takes as input a request function that defines the logic for sending new task to the server. Contrary to previous example, this time we need to send a `POST` request, i.e. we want to send what the user entered in the input field to the server-side API. Our data is a `Task` record containing just `name`. Also, since we send it as the `application/json` content type, we must stringify the payload before passing it as the body of the request.

```tsx{3,5,7-14,19-20}
import React from 'react';
import { useForm } from "react-hook-form";
import { useMutation } from "react-query";

import { Task } from 'Task/Shape';

const request = (data: Task) =>
  fetch('/_api/task', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data),
  })

export const TaskInput: React.FC<{}> = () => {
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
```
