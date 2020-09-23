---
pos: 2
title: Uploading a File
---
# Uploading a File

Define a `POST` route for uploading files, let's call it `/upload`:

```js
import { Routes, routing } from 'kretes';

const { Route } = routing;

const routes: Routes = [
  Route.POST('/upload', request => {
    const { files } = request;
  }),
]
```

Kretes automatically parses requests with uploaded files (`multipart`) and makes them available as the `files` property in the request object that's passed to the handler.

For simplicity, you we can also destructure directly in the signature of the handler function:

```js
import { Routes, routing } from 'kretes';

const { Route } = routing;

const routes: Routes = [
  Route.POST('/upload', ({ files })=> {

  }),
]
```

Each uploaded file is available as a property of the `files` object. This property name is being specified by the user when doing a request, it is usually set in the form with the `name` attribute.

```html
<form action="/upload" method="post" enctype="multipart/form-data">
  <label for="avatar">Choose a profile picture:</label>
  <input type="file" name="avatar" accept="image/png, image/jpeg">
</form>
```

Once the form is submitted to `/upload` the uploaded file will be available as the `avatar` property on the `files` object. We can further desctructure it to get the actual file name along with its content available under the `data` property.

```js
import { Routes, routing } from 'kretes';

const { Route } = routing;

const routes: Routes = [
  Route.POST('/upload', ({ files }) => {
    const { name, data } = files.avatar;
  }),
]
```

Finally, we can store the uploaded file on the server using the built-in `writeFile` function from the `fs` module.

```js {1,2}
import { promises as fs } from 'fs';
import { join } from 'path';
import { Routes, routing } from 'kretes';

const { Route } = routing;

const routes: Routes = [
  Route.POST('/upload', async ({ files }) => {
    const { name, data } = files.avatar;

    await fs.writeFile(join(cwd, 'static', 'uploads', name), data);

    return 'File successfully uploaded.'
  })
]
```