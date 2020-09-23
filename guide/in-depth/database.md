---
pos: 2
title: Database
---
# Database

More often than not an ORM brings too much to complexity to just get some data out of a database. Kretes favors the query builder approach based on the
[Sqorn](https://sqorn.org/) library. With this methods, you can write SQL queries that are integrated in your code as the regular JavaScript data structures.

For the rare cases, when you need the direct power and expressivness of SQL you can use [PgTyped] to write these SQL queries in separete files.

## Configuration

The database configuration is stored `config/default.js`.

To start using the database integration you need to import the `database` namespace:

```js
import { database } from 'kretes';
```

For convenience, let's create the `db` alias:

```js
import { database as db } from 'kretes';
```

## Basic Queries

Let's see how we can perform simple SQL queries:

### `SELECT` Queries

Get all elements with all columns from `widgets` table; equivalent to `select * from widgets`:

```js
const results = await db`widgets`;
```

Get all elements with all some columns from `widgets` table; equivalent to `select id, name from widgets`:

```js
const results = await db`widgets`.returning('id', 'name');
```

Get a single element from `widgets` table by `id`:

```js
const result = await db`widgets`.where({ id })
```

### `INSERT` / `UPDATE` Queries

Insert a single element into `widgets` table:

```js
await db`widgets`.insert({ name: 'Widget 1', amount: 2 })
```

Insert few elements at once into `widgets` table:

```js
await db`widgets`.insert([
  { name: 'Widget 1', amount: 2 },
  { name: 'Widget 2', amount: 7 },
  { name: 'Widget 3', amount: 4 }
])
```

Update an existing element (identified by `id`) in `widgets` table:

```js
await db`widgets`.where({ id: 2 }).set({ name: 'Widget 22' })
```

### `DELETE` Queries