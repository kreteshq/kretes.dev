---
pos: 3
title: 3. Using TypeScript
description: >
  Defining the shape of date using TypeScript interfaces
images:
- typescript.svg
prev:
  title: Setup 
  path: tutorial/creating-the-application/
next:
  title: Prototyping with HTML 
  path: tutorial/html-prototype/
---

# TypeScript Shape

Since we use TypeScript, let's define the shape of our data. For now, it will be just the `Task` shape that consists of a `name` and the `done` status.

Use the `generate` command to generate a shape scaffold for our Task:

```
kretes generate type Task
```

This will create `<project root>/types/index.ts` with an empty `Task` interface :

```ts
export interface Task {
};
```

Let's adjust it to our use case by specifying two fields `name` and `done`. The `done` field has a question mark (`?`) at the end. This means it's an optional field.

```ts
export interface Task {
  name: string;
  done?: boolean
};
```
