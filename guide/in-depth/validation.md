---
pos: 4
title: Validation
---
# Validation

```ts
import { Handler, response, Middleware, Pipeline, request } from 'kretes';

const { validate } = request;
const { OK } = response;

const validator: Middleware = validate({ 
  name: { 
    type: String, 
    required: true, 
    length: { min: 3, max: 5 } 
  }
})

const handler: Handler = async (request) => {
  return OK('X');
}

export const browse: Pipeline = [
  validator, 
  handler
]
```