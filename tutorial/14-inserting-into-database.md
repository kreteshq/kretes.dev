---
pos: 14
title: 14. Inserting into Database
description: >
  Persisting tasks on the server via REST API
---

# Inserting into Database

Finally, we can define the API handler for adding new tasks to the collection. We need the `create` handler in the `Task` controller. Put the following code in `features/Task/Controller/create.ts`:

```ts
import { Handler, response, database as db } from 'kretes';

const { Created } = response;

export const create: Handler = async ({ params }) => {
  const { name } = params;

  await db.from('task').insert({ name });

  return Created();
}
```

Refresh the page, and try adding a new task via the form. The new task should be added to the list and it won't disapear, even if you refresh the page in the browser, since the tasks are stored in the database on the server.

Optionally, let's clear the input field once a new task is added. Modify the file `features/Task/View/Input.tsx` as follows:

```ts
const { register, handleSubmit, errors, reset } = useForm<Task>();

const onSubmit = handleSubmit(data => {
  mutation.mutate(data)
  reset();
});
```
