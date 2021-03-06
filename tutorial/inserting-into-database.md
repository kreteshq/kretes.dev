---
pos: 13
title: 13. Inserting into Database
description: >
  Persisting tasks on the server via REST API
images:
- postgresql.svg
---

# Inserting into Database

Finally, we can define the API handler for adding new tasks to the collection. We need the `create` handler in the `task` controller. Put the following code in `<project root>/site/_api/task/create.ts`:

```ts
import { Handler, database as db } from 'kretes';
import { Created } from 'kretes/response';

export const create: Handler = async ({ params }) => {
  const { name } = params;

  await db.from('task').insert({ name });

  return Created();
}
```

Refresh the page, and try adding a new task via the form. The new task should be added to the list and it won't disapear, even if you refresh the page in the browser, since the tasks are stored in the database on the server.

Optionally, let's clear the input field once a new task is added. Modify the file `<project root>/components/TaskInput.tsx` as follows:

```ts
const { register, handleSubmit, errors, reset } = useForm<Task>();

const onSubmit = handleSubmit(data => {
  mutation.mutate(data)
  reset();
});
```
