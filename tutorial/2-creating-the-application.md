---
pos: 2
title: Create App
description: >
  Initializing projects with templates
---

Install Kretes globally. Be sure, you have version `0.80.1` or higher

```
npm i -g kretes
```

You can check the Kretes version with the `-V` option:

```
kretes -V
```

During installation, Kretes adds the `ks` alias for convenience so that you can write `ks <command>` instead of `kretes <command>`. In this tutorial we will be always using the full `kretes` name, but keep in mind you can reduce that to the shorter `ks` alias if needed.

---

Create a new applicaiton using the React.js template

```
kretes new taski.app --template=react
```

The React.js template comes with the following libraries:

* [react-query](https://react-query.tanstack.com)
* [react-hook-form](https://react-hook-form.com)
* [wouter](https://github.com/molefrog/wouter)
