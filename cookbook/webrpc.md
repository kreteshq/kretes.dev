---
pos: 4
title: Using WebRPC
description: WebRPC is a REST/GraphQL alternative for the front-end/back-end communication
---

# Using WebRPC

Kretes uses only one programming language - TypeScript - for the entire application, whether it is front-end or back-end. By using TypeScript across the board, it is now possible to share code (with some limitations) between these two layers. For example, thanks to TypeScript, it is possible to define the shape of the application data **once**, no matter if it's client-side or server-side - something that's relatively more complicated if there are different programming languages on the front-end & back-end.

We can now start building upon this convenience of having TypeScript everywhere, specifically in the context of communication between the front-end part and the back-end part. Usually, we use the HTTP protocol (with something like REST or GraphQL) to get data from the server and to display it on the client. The moment the data crosses the layers we somehow loose the information about the shape of what's being transferred. We must then manually re-create that information and incorporate it again on the client.

The front-end/back-end communication doesn't have to be like that. We could define the signatures for services available on the server once and have this information automatically available on the client. Let's take an example. We define a service available server-side along with its signature using a TypeScript interface:

```ts
import { Task } from 'Task';

export interface TaskService {
  browse(): Promise<Array<Task>>;
}

export default class implements TaskService {
  async browse(): Promise<Array<Task>> {
    return ... // concerete implementation
  }
}
```

This `TaskService` interface could be consumed automatically on the client, so that we could not only invoke a method directly, but we would also have its signature and its return type available. In other words, on the client, we could write this:

```ts
await TaskCaller.browse()
```

instead of invoking the HTTP request manually that requires to specify all the connection parameters and to handle the response parsing:

```ts
const response = await fetch('/path/to/task/endpoint', {
  method: 'GET',
  headers: ...,
  body: JSON.stringify(...)
})

const result = await response.json();
```

Luckily, this is works out-of-the-box in Kretes. Once you define the services along with their interfaces (on per-feature basis), the service caller is automatically generated client-side with all the service methods and their types.

```ts
import TaskCaller from 'Task/Caller'

const tasks = await TaskCaller.browse();
```

(The `Task/Caller` module is generated automatically)

This approach isn't new. We can find it in the Remote Procedure Call (RPC) and with technologies such as CORBA, Web Services/SOAP. It is a much more unified, thus simpler approach than managing HTTP requests and responses directly. In Kretes, it is called WebRPC as still uses HTTP for simplicity. It is however relatively simple to switch the transport layer to a binary solution.

