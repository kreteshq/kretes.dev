---
title: How Kretes Works
pos: 1
---
# How Kretes Works

## Application Setup

* scalable, maintainable directory structure
* out-of-the-box support for TypeScript
* compatible with major UI libraries such as Vue.js, React.js, Preact or Svelte
* auto-generated RESTful APIs from the project directory structure (naming convention)
* PostgreSQL integration and setup without manual installation

## Development

* instant __Hot Module Replacement__
* on-the-fly, on-demand, extremely fast TypeScript compilation using [ESBuild](https://github.com/evanw/esbuild)
* no bundling during development by using ECMA Script modules, also known as __unbundled development__
* decoupled development dependencies from user dependencies

## Production

* efficient bundling using Rollup
* simpler Continuous Integration (CI) process
* reproducible environment builds

## Deployment

* small Docker containers created by [Nix](https://nixos.org/)

## Architecture

* TypeScript across the entire application: one ecosystem, one tooling, one skillset
* A SQL Query Builder instead of an ORM as [SQL is one of the most valuable skills](http://www.craigkerstiens.com/2019/02/12/sql-most-valuable-skill/)
* A SQL-like, data-driven abstractions for the database integration
* [Sqorn](https://sqorn.org/) for the database integration which provides a SQL-like abstractions right inside JavaScript
* Tailored for PostgreSQL
* Unwilling to support NoSQL: ([Thank you for your help NoSQL, but we got it from here](https://www.memsql.com/blog/why-nosql-databases-wrong-tool-for-modern-application/))