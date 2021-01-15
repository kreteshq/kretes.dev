---
pos: 12
title: Database Setup
description: >
  Setting up a relational database
---

# Database Setup

Before we will be able to implement the API handler for accepting new tasks, we need to define a single location for storing them. This way both handlers will have access to the same data location. In other words, we need a database.

Let's open the `config/default.json` and define the name of our database under the `db` key.

```json{2-4}
{
  "db": {
    "database": "acme"
  },
  // ...
}
```

Now, we need to start the database engine. There are several ways to do that depending on the platform. Check [the PostgreSQL documentation](https://www.postgresql.org/download/) for details. By default, the PostgreSQL RDBM will start on the port `5432` and Kretes will automatically connect to it. If there is no database engine running, Kretes will print an error, but start anyway; you will be able to continue developing the app, but without the access to the database.

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

Then, run the database setup method:

```
kretes database setup
```

You can test if the data was correctly added to the database with `psql` that comes with the PostgreSQL installation. Point it to your database (`acme` in our case)

```
psql acme
```

and run a SQL query on the `task` table:

```sql
select * from task;
```
