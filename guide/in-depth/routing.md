---
pos: 5
title: Routing
---
# Routing

## Overview

Routing defines how an application’s endpoints (URIs) respond to client requests. It's a set of routes When building web applications we can handle routing on the server and on the client. Those two approaches are not exclusive and are usually used together. On this page we describe the server-side routing as client-side routing is tightly related to a specific UI solution.

In Kretes we use the existing data structures from JavaScript to define the application routing, i.e. the data-driven approach. The routing is a collection of routes. Each route is a three-element tuple that consists of a path name, its characteristics and a point of nesting.

```js
const routes: Routes = [
  ["/api", {},
    ["/nested", {},
      ["/planet", {
        middleware: [ ... ],
        GET: () => {...},
        POST: () => { ... },
      }]
    ]
  ],
];
```

At a first glance, this may look complicated, but the idea is to provide flexibility when defining routes in different scenarios. To make the routing definition less verbose, Kretes provides a set of helpers under the `Route` name.

```js
const routes: Routes = [
  Route.GET("/api/nested/planet", () => { ... })
  Route.POST("/api/nested/planet", () => { ... })
  Route.PATCH("/something/else", () => { ... })
]
```

Behind the scenes, the same tuple structure is generated, but using the `Route` helper the route definitions are slightly more concise.

All Kretes server routes are defined in the `routes.ts` file that is located in the `config/server` directory. This file is automatically loaded by the framework.

The application listens for requests that match the specified paths and methods, and when there is a match, it triggers the specified handler function.

## Router HTTP Methods

The router allows you to register routes that respond to any HTTP verb:

```ts
Route.GET(<path>, <handler>)
Route.POST(<path>, <handler>)
Route.PUT(<path>, <handler>)
Route.PATCH(<path>, <handler>)
Route.DELETE(<path>, <handler>)
Route.OPTIONS(<path>, <handler>)
```

At times you may need to define a route that responds to multiple HTTP methods. You can use the `Route.match` for that:

```ts
Route.match(['GET', 'POST'], '/', () => { ... })
```

## Handlers

Contrary to Express.js (and similar frameworks), a **handler** in Kretes is a *one argument* function. This argument is the incoming request. The return value is used by Kretes to create an HTTP response.

```js
// An example of a handler
const browse = request => {
  return { ... } // <- an HTTP response
}
```

In Express, and the majority of other Node.js frameworks, handlers take two arguments. The first one is the request and the second one is the response.

In Kretes, the response is simply everything that is being returned by the handler. This way, it may be slightly more natural to think about the process of handling requests and generating responses: handlers are functions, which take requests as their input and produce responses as their output. The response is represented as a JavaScript object which must have at least the `body` key.

```js
const fetch = request => {
  return { body: 'Hello, Kretes!' }
}
```

The return value can be a string. In that case the response is `200 OK` with the `Content-Type` header set to `text/plain`, e.g.

```js
const say = request => {
  return 'This is nice'
}
```

Usually the value returned by a handler is an object with (at least) the `body` property. Optionally, you can also specify the `headers`, `statusCode` or `type` properties. This constitutes the `Handler` type.

```js
import { Handler } from 'kretes';
const fetch: Handler = request => {
  return {
    body: '<h1>Hello World</h1>',
    type: 'text/html',
    statusCode: 200,
    headers: {}
  }
}
```

Kretes uses plain objects (a regular data structure in JavaScript) to represent HTTP responses. That's why we say it's a data-driven (and declarative) approach. This is inspired by the ring library from the
Clojure community.

In some relatively rare cases, the response can be also a stream. Kretes sets the `type` automatically to `application/octet-stream` in that event.


## Wrappers For Common HTTP Responses

It would be arduous to create an object with the specific fields each time an HTTP response is needed. Kretes provides convenient wrappers in that situation.

Instead of writing:

```js
import { Handler } from 'kretes';

export const fetch: Handler = request => {
  return {
    body: '<h1>Hello World</h1>',
    type: 'text/html',
    statusCode: 200,
    headers: {}
  }
}
```

you can use the `HTMLString` wrapper and write this:

```js
import { Handler } from 'kretes';
import { HTMLString } from 'kretes/response';

export const fetch: Handler = request => {
  return HTMLString('<h1>Hello World</h1>')
}
```

### Set The Preferred Response Format

Kretes determines the preferred response format from either the HTTP `Accept` header or `format` query string parameter, submitted by the client. The `format` query parameter takes precedence over the HTTP `Accept` header.

Based on the preferred format, you can construct actions that handle several possibilities at once using just the JavaScript's `switch` statement - no special syntax needed.

```js
const browse = ({ format }) => {
  // ... the action body

  switch (format) {
    case 'html':
      // provide a response as a HTML Page
      return HTMLString(...)
    case 'csv':
      // provide a response as in CSV format
      return CSVPayload(...)
    default:
      // format not specified
      return JSONPayload(...)
  }

}
```

## Redirects

You may need to define a route that redirects to another path. This can be achieved using the response wrappers from the `response` namespace.

```ts
import { response, routing } from 'kretes';

const { Redirect } = response;
const { Route: { GET } } = routing;

const routes: Routes = [
  GET("/from-here", Redirect("/to-here"))
]
```

## View Routes

If a route only needs to return a view, you can combine the `Route.GET` with the `Page` response wrapper. The `Page` wrapper accepts a view name as its first argument and an optional list of parameters as its second argument.

```ts
import { response, routing } from 'kretes';

const { Page } = response;
const { Route: { GET } } = routing;

const routes: Routes = [
  GET("/hey", Page("hey-page", { name: "Kretes" }))
]
```

## Route Parameters

There are three kinds of parameters in a web application:

1. query string parameters, sent as part of the URL after `?`
2. body parameters, sent as part of the request `body`, referred to as POST data (usually comes from an HTML form or as JSON)
3. path segment parameters, sent as part of the route path, prefixed with `:`


Kretes does not make any distinction between these parameters, all of them are available in the router handler as the `params` object.

```ts
const handler = ({ params }) => {
  const { name } = params;
}

const routes: Routes = [
  Route.GET('/welcome/:name', handler)
]
```


## Reusable workflows / Middlewares

Handlers can be composed from simple functions so that the shared bevahior can be extracted into reusable chunks of code. Those functions are equivalent to Express.js/Koa middlewares. 

### Per-Route Middlewares 

In order to define a workflow, you need to map an array of functions with a handler at the end to the specific path.

```js
const routes: Routes = [
  ['/dashboard', {
    middleware: [middleware_A, middleware_B],
    GET: handler
  }]
]
```

Those functions are composed from left to right so that the declaration above is equal to the following one:

```js
const routes: Routes = [
  ['/dashboard', {
    GET: middleware_A(middleware_B(handler))
  }]
]
```

You can also use the `Route` helper to make the route definition more concise:

```js
const routes: Routes = [
  Route.GET('/dashboard', handler, [middleware_A, middleware_B]);
]
```

Such composition creates workflows that can contain validation, logging, profiling, permission checking or throttling. Here's an example of a simple validation that checks if the request parameters contain the `admin` field of the type `String`.

```js
import { validate } from 'kretes/request';

const handler = ({ params: { admin } }) =>
  `Admin param (${admin}) should be absent from this request payload`

const routes: Routes = [
  Route.GET('/request-validaton', handler, [validate({ name: { type: String, required: true } })])
]
```

Those workflows are **local** for the particular path, contrary to Express.js that wraps every middleware around every path. 


### Global Middlewares 

Global middlewares are middlewares that will be executed for every route. You can define them inside `config/server/middlewares.ts`. It's a list of functions that defines their execution from top to bottom.


```js
import { Middleware } from "kretes";

export const middlewares: Middleware[] = [
  handler => {
    // here you can define the state
    // in between the request-response cycle
    let id = 0, sequence = () => id++

    return request => {
      request.id = sequence()

      return handler(request)
    } 
  }
];
```

