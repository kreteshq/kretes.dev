---
pos: 2
title: Create App
description: >
  Initializing projects with templates
---

# Project Init

Install Kretes globally. Be sure, you have version `0.85.0` or higher

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

Once the project initialization is over, switch to the project directory

```
cd taski.app
```

Open it using your editor of choice and start the application in development mode using the `start` command:

```
kretes start
```

From now one you shouldn't need to restart the application while developing. There may be occasional quirks, but I'm working to eliminate them. Feel free to let me know anytime if you stumble on something.

The application starts on the port `:5544`. In your browser, open http://localhost:5544 to see it.
