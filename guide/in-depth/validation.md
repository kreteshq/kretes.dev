---
pos: 4
title: Validation
---
# Validation

```ts
import { Handler, response, Middleware, Pipeline, request } from 'kretes';
import { validation } from 'kretes/request';
import { OK } from 'kretes/response';

const validator: Middleware = validation({ 
  name: { 
    type: String, 
    required: true, 
    length: { min: 3, max: 5 } 
  }
})

const handler: Handler = request => {
  return OK('Displayed only if the validation passes');
}

export const browse: Pipeline = [
  validator, 
  handler
]
```