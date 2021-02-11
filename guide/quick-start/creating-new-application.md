---
title: Creating a New Application
pos: 2
prev:
  title: Installing Kretes
  path: guide/quick-start/installing
next:
  title: Starting the Application
  path: guide/quick-start/starting-application
---
# Creating a New Application

## `New Project` Command

From the command palette, select `Kretes: New Project`. Set the project name and select a directory in which the project should be created. A new editor window will be created with the project scaffold.

![Kretes Zap: New Project](/images/zap/kretes-new-project.gif)

> A Kretes application scaffold can be also generated from the command line using `kretes new my-project`. By default, the `new` command will also install the project dependencies. You can skip that using the `--no-npm-install` option.

## Directory Structure

The application directory has a number of auto-generated files and folders that make up the structure of a Kretes application. Check the [Directory Structure](/docs/guide/in-depth/directory-structure/) section for details.


## Database Setup

A Kretes application requires a database. You probably wouldn't need a web framework if you wanted to create a web application without a database. Kretes supports only [PostgreSQL](https://www.postgresql.org/) as it heavily uses its unique features to help create performant web applications faster.

Kretes will launch the PostgreSQL process and create a database automatically behind the sceens once you start the application. This is managed using the Nix package manager so that you don't have to worry, which PostgreSQL version to use or how to connect to the process to create a database.

The generator sets the database name to the application name. You can change that setting in `config/default.yml`.



