---
title: What is Kretes
pos: 0
---
# What is Kretes

Kretes is an integrated programming environment for [TypeScript ](https://www.typescriptlang.org/) & [Node.js](https://nodejs.org/en/) (soon [Deno](https://github.com/denoland/deno)), built on top of [Visual Studio Code](https://code.visualstudio.com/). Its goal is to help create software faster. It consists of three elements:
1. a *batteries included* web framework similar to Rails/Django (done in about 50%)
1. a tightly adapted, customized UI layer built inside of the VS Code editor (in progress)
1. infrastructure (hosting, DB) with seamless deployments built on top of WebAssembly and PostgreSQL (soon available)

It all just works, great for seasoned developers and new programmers. You can focus on solving the actual business needs of the application you're building instead of fighting [the accidental complexity](https://wiki.c2.com/?AccidentalComplexity).

## Rationale

### Battery-included

Kretes is being built with battery included approach in mind, i.e. it comes with a (eventually large) library of useful, built-in modules that are developed in a __coherent way__. This stands in direct opposition to how Express.js (or Koa) based applications are built. Kretes tries to formalize conventions and eliminate valueless choices by providing solid defaults for building web applications. This way the tool intends to increase the programmers productivity.

### Inspired by Self

Kretes explores the ideas introduced by [the Self programming language](https://selflanguage.org/) (Sun Microsystems '95) and aims to bring them to the current age. It will be a semi-visual tool for building web apps using the same programming language (TypeScript) on the client & on the server. Such an approach blurs the line between front-end & back-end, which fits the broader industry shift to *full-stack* teams that work from front-end to back-end.

### Just TypeScript

By using TypeScript across the board, there is just one ecosystem, just one tooling, just one way of hiring for software teams to learn and integrate. In other words, it is a much more unified, thus simpler approach. Moreover, it is now possible to *share* (with some limitations) not only code, but certain coding practices. As a result this solution not only simplifies, but also speeds up the development process.

## In a Nutshell

There are two essential ways to build a web application in Kretes. You can either go with a traditional server-side application, or you can focus on client-side by creating a single-page application (SPA). Those two approaches can be also combined.

Kretes can be also used as a replacement for Express or Koa to build server-side only applications, but it also goes beyond that by providing opinionated choices to other layers in the stack (view, ORM, etc) required to build a fully functional web application.

As a secondary goal, Kretes tries to minimize the dependencies. It uses external packages only if absolutely necessary (e.g. security, OS abstractions etc).

### Organization

* Folder-by-feature directory structure

### Routing

* Router with data-driven abstractions for HTTP responses and handlers
* Wrappers for HTTP responses (e.g. `OK`, `NotFound`, etc)
* Workflows created via the function composition
* Implicit built-in REST endpoint from the directory structure
* Implict built-in GraphQL endpoint via PostGraphile

### Database

* Use of SQL via [PgTyped](https://github.com/adelsz/pgtyped)
* 100% typesafe query builder
* Reluctance to support ORMs, but flexible to do so if needed

### Client-side

* Use of ES modules via [Snowpack](https://www.snowpack.dev/) instead of bundling (for development)