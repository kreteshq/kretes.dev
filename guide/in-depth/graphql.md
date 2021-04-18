---
pos: 7
title: GraphQL
---

# GraphQL

Kretes automatically creates a GraphQL endpoint using the shape of your database. This feature is built on top of Postgraphile.

At `/_graphiql` you can access GraphiQL, a graphical interface to your application's GraphQL endpoint. Assuming there is a table called `widget` in your database, you can use the following GraphQL query to get all those widgets

```
{
  widgets {
    nodes {
      name
    }
  }
}
```

Click on the **Docs** on the right side to explore the available schema so that you can make queries and mutations based on your database.