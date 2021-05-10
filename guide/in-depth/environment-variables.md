---
pos: 3
title: Environment variables 
---

# Environment variables

You can set environment variables inside the `.env` file located in the project's root.

```bash
PORT=5544
API_KEY=8dhsk9k30sksdf77294j5
```

By default, all enviroment variables are **only** visible on the server-side. If you want to have a variable visible also on the client, prefix its name with `PUBLIC_`.

```bash{3}
PORT=5544
API_KEY=8dhsk9k30sksdf77294j5
PUBLIC_ANOTHER_ONE=zzxxxyyy222
```