---
pos: 1
title: 1. Specification
description: >
  A spec describing what we are going to build in this tutorial
---

# Specification

![Full-Stack TypeScript Example: Taski.app](/images/taskiapp.jpg#center)

[Live Demo](https://taski.app)

In this tutorial we will be writing a fully featured [TypeScript](https://www.typescriptlang.org/) application using Kretes. In other words, we will cover the whole process of building a **full-stack TypeScript** application in a comprehensive way - we could say from A to Z. This tutorial will cover not only the front-end part, but also the back-end part and everything in-between, from setting up the database through designing the database schema, exposing an API using both REST & GraphQL endpoints, till the application deployment.

This tutorial will show you how to connect different application layers to create a fully functioning web application. This approach aims to give you the big picture of building web applications so that you have a basic understandings of all the elements and techniques needed for creating your own application from scratch, or rather from A to Z.

## Goal

We will build yet another task management application. I decided to showcase creating another task manager because the notions of such application are simple and well understood. You already know what a task is, you know that you can group or filter them, or that you can mark tasks as done. You probably used a task manager yourself at some point. As a result, instead of spending time on explaining the business features and their outcomes, we can be practical and focus on building the actual app.

## Technology Stack

On the back-end we will be using [Node.js](https://nodejs.org/en/) an open source runtime built on Chrome's V8 JavaScript engine for creating both server-side apps, as well as JavaScript code run outside of the browser environment, e.g. command-line tools.

On the front-end we will be using [React.js](https://reactjs.org/), an open-source, front end, JavaScript library for building user interfaces or UI components.

For the database layer, we will use [PostgreSQL](https://www.postgresql.org/), one of the most popular relational databases out there. We will design the database schema, create tables and make relations between them. We will also see how to use PostgreSQL as a NoSQL solution and how it compares to databases such as MongoDB et al.

Instead of writing CSS by hand we will use [Tailwind CSS](https://tailwindcss.com/),
a CSS framework packed with utility classes like `flex`, `pt-4`, `text-center` and `rotate-90` that can be composed to rapidly build modern websites, directly in HTML markup.

Finally, Kretes will put all those tools together in a ready to use solution (a boilerplate on steroids) so that you can focus on building the actual application and its business logic, instead of spending time on trying to put things like TypeScript, Node.js, React.js or PostgreSQL together manually.

## Future

This tutorial is meant to be modern. At some point I may switch certain things in our stack. For example, we might try to replace Node.js with [Deno](https://deno.land/) or React.js with the [Solid.js](https://github.com/ryansolid/solid) framework, etc.

The idea is to track the current trends & tendencies in web development instead of using older, already established ways of creating web applications, and also to give you an early example how to use these modern solutions to build full-stack TypeScript applications. In other words, our goal here is to explore new or improved ways of building web applications.


