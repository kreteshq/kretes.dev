---
pos: 14
title: Docker Image
description: >
  Building a Docker image for a Kretes app
images:
- docker.png
prev:
  title: Inserting into Database 
  path: tutorial/inserting-into-database/
next:
  title: Using Tailwind CSS 
  path: tutorial/using-tailwindcss/
---

# Building a Docker image

Kretes comes with a built-in Docker integration. Each Kretes project has a `Dockerfile` that configures the Docker image specifically for Kretes.

You can build a Docker image by running the Kretes' `docker build` command:

```
kretes docker build
```
