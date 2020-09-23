---
title: Installing
pos: 1
next:
  title: Creating a New Application
  path: guide/quick-start/creating-new-application
---
# Installing Kretes

## Prerequisites

Kretes uses [the Nix package manager](https://nixos.org/) to simplify the management of the application dependencies. This not only makes the development environment easier to manage, but it also can substantially help organize dependencies when deploying to production.

Once Nix is installed, there will no longer be needed to install any dependencies manually, including things like setting up and configuring the database and its plugins. From now on everything needed by Kretes to run your application will be automatically installed and configured behind the scenes.

Note, that the installation of **the first dependency** in Nix can be a bit long, but afterwards it will be quick again.

### Linux

```
curl -L https://nixos.org/nix/install | sh
```

### MacOS

```
sh <(curl -L https://nixos.org/nix/install) --darwin-use-unencrypted-nix-store-volume
```

### Windows

Windows requires the Windows Subsystem for Linux (WSL). This needs to be manually activated using Powershell (with administrator privileges):

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

Here's a guide how to [install Nix on Windows](https://nathan.gs/2019/04/12/nix-on-windows/) by Nathan Bijnens

---

Once Nix is installed, try to install the PostgreSQL package:

```
nix-shell -p postgresql_12
```


## Using VS Code (Recommended)

Install the [Kretes extension](https://marketplace.visualstudio.com/items?itemName=kretes.kretes) from the VS Code Marketplace.

![Kretes Zap: Installing the Extensions](/images/zap/kretes-installing.gif)

> If you enjoy the process, please rate the extension. It will help raise the project visibility. If there's something wrong, please let me know before, so I could fix. It's in human nature to make errors and I'd appreciate your communication so we could make it better together!

## Using CLI

Install `kretes` globally to use the CLI commands

```
npm install -g kretes
```


Open the application in Visual Studio Code

```
code my-project
```
