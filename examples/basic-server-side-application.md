---
pos: 1
title: Basic Server-Side Only Application
---
# Basic Server-Side Only Application

This is an example of a basic server-side application Kretes. Save it to a file e.g. `server.js`, run it with `node server.js` and visit the application `https://localhost:5544`.

> *Note* Don't forget to install `kretes` by adding it to `package.json` in your project directory followed by `npm install`. If you're starting from scratch, use `npm init` or (better) `kretes new` described below.

```ts{2-3}:app.ts
import Kretes, { Routes, response, routing } from 'kretes';
const { OK } = response;
const { Route: { GET, POST } } = routing;

const app = new Kretes();

const routes: Routes = [
  // implicit `return` with a `text/plain` response
  GET('/', _ => 'Hello Kretes'),

  // explicit `return` with a 200 response of `application/json` type
  GET('/json', _ => OK({ a: 1, b: 2 })),

  // set your own headers
  GET('/headers', _ => ({ body: 'Hello B', statusCode: 201, headers: { 'Authorization': 'PASS' } })),

  // request body is parsed in `params` by default
  POST('/bim': request => `Hello POST! ${request.params.name}`),
]

app.start({ routes, port: 5544 });
```

This example shows a regular, server-side application. You define routes as a plain object, which makes composition easier. In contrast to Express, Kretes handlers only take HTTP `request` as input and always return an HTTP response: either defined explicitly as an object with `body`, `status`, etc keys, or implicitly with an inferred type e.g. `text/plain` or as a wrapping function e.g. `OK()` for `200`, or `created()` for `201`.
