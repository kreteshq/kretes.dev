---
pos: 5
title: Background Processing
---
# Background Processing

## Overview

* A simple and efficient background processing for Node.js out of the box
* The task/job queues are handled by PostgreSQL and stored in the same database as the application itself
* CLI tools for starting the background process and scheduling tasks

Kretes provides a simple and efficient background processing in Node.js using the [graphile-worker](https://github.com/graphile/worker) package.

The task are written in TypeScript and put by name on the task queues along with the necessary payload.

```ts
const SendEmail: Task = async input => {
  const { name } = input;

  console.log(`Hello, ${name}`);
}
```

By convention, the task names in Kretes are written in Camel Case with the first letter uppercase: think, this is a `SendEmail` task.

Each task must be placed in a separate file within the `tasks/` directory at the root of your Kretes project, e.g. `tasks/SendEmail.ts` for the task above.

This background processing mechanism is integrated directly into PostgreSQL (it requires PostgreSQL 10 or higher). You may wonder why not use Redis or something similar. The answer is **simplicity**. Putting the job/task queues in a relational database is minimally less performant than Redis while providing a significant convenience for the application maintenance: there is less elements to install, manage and configure when running your application.

Also, you can use SQL, a familiar interface to query about tasks & queues statuses instead of learning yet another domain specific language as, for example, in the case of Redis. Simplicity for the win.

You can run the background processing mechanism using the `background` command:

```bash
kretes background
```

There is also a convenient `bg` alias for that:

```
ks bg
```

From now on you can schedule tasks. The process of scheduling consists of putting the task name along with its input payload on a task queue. This is usually done from within your application in response to some activity, e.g. you send a welcome email once a user registers, etc.

```ts
Background.schedule({ task: SendEmail });
```

For some tasks you may need to provide some input data (the payload) so that they execute properly:

```ts
Background.schedule({
  task: SendEmail
  payload: { to: 'admin@example.com' }
});
```

By default the task is scheduled on a new queue, i.e. the queue name is randomly generated. This means that the worker executes tasks in parallel if there is enough throughput (CPUs).

You may need to force an execution order for certain tasks. In this case you need to schedule those tasks on the same queue so that they run serially:

```ts
Background.schedule({
  task: SendEmail,
  queue: Queue.for('email')
});
```

Lastly, you can schedule tasks via the CLI. This is useful while in development to quickly test if tasks execute as planned:

```bash
kretes background schedule <name> [payload]
```

The `name` parameter is a mandatory task name, e.g. `SendEmail` while the `payload` parameter is an optional JSON payload as `string`.

```bash
ks bg schedule SendEmail '{ "to": "admin@example.com" }'
```

