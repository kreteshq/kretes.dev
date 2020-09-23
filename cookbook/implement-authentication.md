---
pos: 3
title: Implementing Authentication
---
# How To Implement Authentication

We need two tables in our applications: one for storing users and their passwords, and another one for storing the sessions.

Let's start with the table for users. We will call it `person`:

```sql
create table person (
  id serial primary key,
  name text not null,
  email text not null unique,
  password text not null
);
```

`person` contains the `email` and `password` fields that will be used when logging. Optionally we defined the `name` field. `email` must be `unique` and `password` will be storing a hashed version of the user password.

Next, we need the `session` table to store user sessions:

```sql
CREATE TABLE session (
  id serial primary key,
  token text,
  person_id integer references person(id),
  created_at timestamptz not null default now()
);
```

The `person_id` is the foreign key that references records in the `person` table. `token` will hold the current user session that will be hashed for additional security.

Let's create a controller for registering users. We will use a Kretes functionality to implicitly create REST routes based on the directory structure.

Inside `features/Person/Controller` put the `create.ts` file with the following content:

```js
import { auth } from 'kretes';

const { register } = auth;

export register({ fields: ['email', 'name'] });
```

This action will create users in the `person` table. Kretes provide a convenient template for this functionality as the `register` function from the `auth` module. We only need to define which fields should be extracted from the request params during the user creation. In our case it's `email` and `name` as the `password` field being mandatory is implicitly extracted as well.

Similarly, in `features/Session/Controller` put the `create.ts` file with the following content:

```js
import { auth } from 'kretes';

const { login } = auth;

export login({ finder })
```

This time we need to provide the `finder` function that is mandatory. This function describes how to find a particular user in the database. In our case it will be relatively simple:

```js {2,4}
import { auth, database as db } from 'kretes';

const { login } = auth;

const finder = async ({ email }) => await db.from('person').where({ email });

export login({ finder })
```

Let's create a simple HTML page that will be protected using our authentication mechanism. In `features/Base/Controller/` create `browse.ts`:

```js
import { Handler, response } from 'kretes';

const { OK } = response;

export const browse: Handler = _ => {
  const records = ['Widget 1', 'Widget 2', 'Widget 3'];
  return OK(records);
}
```

When visiting the `/base` the action above will return a list of three widgets as `application/json`. Let's force authentication for this endpoint using another built-in function `authenticate`.

```js {3,10}
import { Handler, auth, response } from 'kretes';

const { OK } = response;
const { authenticate } = auth;

const browse: Handler = _ => {
  const records = ['Widget 1', 'Widget 2', 'Widget 3'];
  return OK(records);
}

export [authenticate, browse];
```

The array syntax designates the function composition, i.e. `[authenticate, browse]` is equivalent to `authenticate(browse)`. In other words, we compose functions one on another from left to right.

Finally, we can test the whole process. Let's start by registering a user:

```
http POST :5544/user email=user@domain.com password=test1234 name=User
```

We can now log in using the password provided during the registration:

```
http POST :5544/session email=user@domain.com password=test1234
```

As a response we will receive a `token` than can be used to pass along with requests to authenticate them. This token can be passed as query params or in the request body. Let's use the query params approach to access the protected route `/base`.

```
http :5544/base?token=<PUT YOUR TOKEN HERE>
```

If the token is correct you should see the list of widgets, otherwise you will get `401 Unauthorized`.


