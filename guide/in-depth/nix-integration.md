---
pos: 3
title: Nix Integration
---

# Nix Integration

Kretes works best with the [Nix](https://nixos.org/) package manager. This simplifies the management of the application dependencies. Nix not only makes the development environment easier to manage, but it also can substantially help organize dependencies when deploying to production.

Once Nix is installed, there will no longer be needed to install any dependencies manually, including things like setting up and configuring the database and its plugins. From now on everything needed by Kretes to run your application will be automatically installed and configured behind the scenes.

Note, that the installation of **the first dependency** in Nix can be a bit long, but afterwards it will be quick again.

## Nix Setup

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

## Verify Nix installation

Once Nix is installed, try to install the PostgreSQL package:

```
nix-shell -p postgresql_13
```

## Nix Shell Integration

You can configure automatic setup of a development environment when entering your project directory.

Install `direnv`:

```
nix-env -i direnv
```

Then, set up the direnv hook for your shell.

### Bash

Add the following line at the end of the ~/.bashrc file:

```
eval "$(direnv hook bash)"
```

### Zsh

Add the following line at the end of the ~/.zshrc file:

```
eval "$(direnv hook zsh)"
```
