---
title: Installing
pos: 1
next:
  title: Creating a New Application
  path: guide/quick-start/creating-new-application
---
# Installing Kretes

Kretes can be installed as a command-line tool, or as a Visual Studio plugin.

## Using CLI

Install `kretes` globally to use its CLI commands

```
npm install -g kretes
```

You can now use the `kretes` command, or `ks` alias, to execute commands.

```
Usage: kretes <command> [options]

Commands:
  kretes new [dir]           Create new project               [aliases: init, n]
  kretes start               Start the application           [aliases: start, s]
  kretes add <pkg>           Add package as project dependency
  kretes install             Install dependencies
  kretes update              Update packages
  kretes upgrade             Upgrade packages
  kretes setup               Setup the development environment
  kretes database [command]  Database operations                   [aliases: db]
  kretes deploy              Deploy the application        [aliases: deploy, de]
  kretes routes              Display routes                         [aliases: r]
  kretes background          Run background processing             [aliases: bg]
  kretes doctor              Run the doctor utility               [aliases: doc]

Options:
  -h, --help     Show help                                             [boolean]
  -V, --version  Show version number                                   [boolean]

Examples:
  kretes new my-project  Create and initialize `my-project` directory
  kretes start           Start the application

for more information, find the documentation at https://kretes.dev

You need at least one command before moving on
```

## As VS Code Plugin

Install the [Kretes extension](https://marketplace.visualstudio.com/items?itemName=kretes.kretes) from the VS Code Marketplace.

![Kretes Zap: Installing the Extensions](/images/zap/kretes-installing.gif)

> If you enjoy the process, please rate the extension. It will help raise the project visibility. If there's something wrong, please let me know before, so I could fix. It's in human nature to make errors and I'd appreciate your communication so we could make it better together!
