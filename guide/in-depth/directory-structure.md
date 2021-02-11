---
pos: 1
title: Directory Structure
---
# Directory Structure

The application directory has a number of auto-generated files and folders that make up the structure of a Kretes application. Here's a quick rundown on the function of each of the files and folders that Kretes generates by default:

| File/Folder | Function |
| --- | --- |
| `components/` | components (React.js, Vue, etc) used to build the applicaton UI (client-side) |
| `config/` | the application config |
| `config/client/` | the client-side config mostly for setting up UI libraries & frameworks (React, Vue.js, etc) + TypeScript config|
| `config/server/` | the server-side config: the routes declaration + TypeScript config |
| `config/default.yml` | the database config, the deployment endpoints and everything else as YAML |
| `controllers/` | controllers define how to handle the server-side routing |
| `site/` | the paths of your app, i.e. everything after the root (`/`) of your domain. It can be client- or server-side (including plain old HTML pages). The structure of this directory maps to the path structure. |
| `static/` | static assets |
| `types/` | a place for TypeScript types i.e. `type` & `interface` definitions |
| `package.json` | npm dependencies needed for your Kretes application |
