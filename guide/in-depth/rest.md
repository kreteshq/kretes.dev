---
pos: 6
title: REST
---

# REST

## Implicit Routes

In Kretes, you can define implicit routes that are derived from the application features. These are called *Resource* routes and they can be configured in `config/server/routes.ts` under the `Resources` key.

```
```

Each resource expects a controller in the `features/<feature name>/Controller` directory. This controller consists of 1 to 5 actions that may be defined in separate files.

Let's say we have a `Game` feature. If we define a `Game` resource as described above, this configuration will implicitly generate the five following routes:

Name | File in `features/` | HTTP Method | Default Path
--- | --- | :---: | ---
**C**reate | `Game/Controller/create.ts` | `POST` | `/game`
**B**rowse | `Game/Controller/browse.ts` | `GET` | `/game`
**F**etch | `Game/Controller/fetch.ts` | `GET` | `/game/:id`
**U**pdate | `Game/Controller/update.ts` | `PUT` | `/game/:id`
**D**estroy | `Game/Controller/destroy.ts` | `DELETE` | `/game/:id`

The action names create a C**BF**UD acronym, an extension of CRUD approach, where we explicitly differentiate between reading a single element and reading a potentially filtered collection of elements.

Actions are responsible to connect the information received from the incoming request to underlaying data in your application (i.e. fetching/saving/updating) in order to produce a corresponding view e.g. a HTML page or a JSON payload.
