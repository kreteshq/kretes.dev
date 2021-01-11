---
pos: 11
title: Optimistic Updates in UI
description: >
  Implementing optimistic updates for React.js UIs
---

# Implementing Optimistic Updates in React.js

The problem now is that we haven't yet defined a handler in our API to accept the payload of a new task. To quickly solve it and move forward, we can cheat a bit by performing a so-called *optimistic update*, i.e. the client state will be updated immediately to reflect the change while the request is being sent to the API.

The important detail is that we don't wait for the API to acknowledge the change. Once the data is fetched again, the state will be synchronized automatically. This approach to changing UIs is relatively popular as it improves the user experience and simplifes the state management.

```tsx{3,19-27}
import React from 'react';
import { useForm } from "react-hook-form";
import { useMutation, useQueryClient } from "react-query";

import { Task } from 'Task/Shape';

const request = (data: Task) =>
  fetch('/_api/task', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data),
  })

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
  );.
}
```

As result, you are now able to add a new task and the change is reflected in UI. This works, however, only for a fraction of time. There is no logic on the server for integrating this new task into the collection on the server. React Query automatically re-fetches our pre-defined collection and overrrides the application state. Thus, the new task disapears after a short moment. We need to now implement API logic for accepting new tasks by appending them to the collection server-side.

The advantage of this approach is that we change UI immediately hoping that the server will also accept the change without any issue - thus, it's *optimistic* and the user sees the change applied right away.
