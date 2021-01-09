---
pos: 13
title: Querying Database
description: >
  Integrating with a database
---

# Querying Database

Once we have the database and the initial data inserted, our server can interact with it by using controllers and their handlers for querying and modyfing the database.

First, let's query the database and return the task collection via the `browse` handler so that it can be displayed in the React.js application. Modify the `features/Task/Controller/browse.ts`

```ts{1,6-7}
import { Handler, response, database as db } from 'kretes';

const { OK } = response;

export const browse: Handler = async ({ }) => {
  const collection = await db.from('task')
  return OK(collection);
}
```

You don't need to restart the application, Kretes takes care of it. The React.js application is now displaying the tasks that are fetched directly from the database on the server.
