---
title: Kretes in a nutshell
pos: 3
---
# Kretes In A Nutshell

## Configuration

* Config has `jsonc` type in VS Code, thus comments are allowed
* Config has a built-in schema
* Kretes uses Nix for setup
* `pnpm` as package manager instead of `npm`

## UI

* You can use React.js, Preact and Vue.js 3
* Svelte support will be available soon

## Database

* PostgreSQL is automatically installed and starts on the port `5454`
* Kretes integrates with SQLTools (the database connection is preconfigured).
* `pgcli` is pre-installed

## Background Processing

* Background processing with typed workers

## Routing

* data-driven routing
* nested routes
* routes for RESTful resources
* webrpc