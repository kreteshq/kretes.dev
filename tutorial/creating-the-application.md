---
pos: 2
title: 2. Setup
description: >
  Setting up the project
prev:
  title: Specification
  path: tutorial/specification/
next:
  title: Using TypeScript 
  path: tutorial/typescript-shape/
---

# Setup

## Prerequisites

### Node.js

Install Node.js via a package manager. Follow the [installation instructions](https://nodejs.org/en/download/package-manager/) at the Node.js website

### pnpm 

[pnpm](https://pnpm.io/) is a fast, disk efficient package manager for JavaScript. It uses a content-addressable filesystem to store packages in a **single place** on your computer - contrary to `npm` or `yarn`, where dependencies are duplicated for each new project. pnpm is also up to 2x faster than `npm` or `yarn`.

### PosgreSQL

Install PostgreSQL, check [the official docs](https://www.postgresql.org/download/) for a method that corresponds to your operating system.

With no configuration, Kretes will be trying to connect to the default PostgreSQL database that is named after your shell username. Go to `config/default.json` to define the database connection for a different database, e.g.

```json
{
  "db": {
    "user": "dbuser",
    "host": "database.server.com",
    "database": "mydb",
    "password": "secretpassword",
    "port": 3211,
  }
}
```

Kretes uses [node-postgres](https://node-postgres.com/), check [its docs](https://node-postgres.com/features/connecting) for all available fields.

### Kretes CLI

Install Kretes globally so that you can access its CLI anywhere.

```bash
npm i -g kretes
```

Be sure, you have version `1.0.0-alpha.44` or higher. You can check the Kretes version with the `-V` option:

```bash
kretes -V
```

<Notice>
During installation, Kretes adds the `ks` alias for convenience so that you can write `ks [command]` instead of `kretes [command]`. In this tutorial we will be always using the full `kretes` name, but keep in mind you can reduce that to the shorter `ks` alias if needed.
</Notice>

## Generate App Structure 

Kretes provides [application templates](/docs/integrations) that combine popular libraries, tools and frameworks to even further speed up the process of creating web applications. In this tutorial we are using React.js on the front end, thus we will generate a Kretes application by passing `react` to the `--template` parameter.

Create a new applicaiton using the React.js template

```
kretes new taski.app --template=react
```

The React.js template configures your application, but also includes a set of libraries that are commonly used when building web applications in React.js:

* [React Query](https://react-query.tanstack.com) is a data synchronization library for React.js that allows to easily integrate front end with back end.
* [react-hook-form](https://react-hook-form.com) is a light, performant library for building forms in React.js
* [wouter](https://github.com/molefrog/wouter) is a tiny (1kB) router for React.js apps that relies on Hooks.
* [clsx](https://github.com/lukeed/clsx) is a tiny (228B) utility for constructing `className` strings conditionally.

## Start App

Once the project initialization is over, switch to the project directory

```bash
cd taski.app
```

Open it using your editor of choice and start the application in development mode using the `start` command:

```
kretes start
```

From now on you shouldn't need to restart the application while developing. There may be occasional quirks, but I'm working to eliminate them. Feel free to let me know anytime if you stumble on something.

The application starts on the port `:5544`. In your browser, open [http://localhost:5544](http://localhost:5544) to see it.
