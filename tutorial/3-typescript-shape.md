---
pos: 3
title: 3. Using TypeScript
description: >
  Defining the shape of date using TypeScript interfaces
---

# TypeScript Shape

Since we use TypeScript, let's define the shape of our data. For now, it will be just the `Task` shape that consists of a `name` and the `done` status. In `features/Task/Shape/index.ts` create the following interface

```ts
export interface Task {
  name: string;
  done?: boolean
};
```
