---
pos: 1
title: Directory Structure
---
# Directory Structure

The directory structure in Kretes is organized around your application **features**, and not **by type**. This means that artifacts, either client-side or server-side are kept together. In other words, this approach groups together entities (classes, functions) that actually work together. This leads to high modularity of your application and better cohesion.

The *Folder-By-Feature* approach makes it easier to find files in your application directory. It is especially visible once your project grows - folder-by-feature is a better long-term approach due its scalability.

To some extend, the *Folder-By-Feature* approach is similar to how recent frontend libraries and frameworks (React, Vue, etc) group together HTML, JavaScript and Stylesheets. In Kretes, this simply goes one step further by applying a similar technique to the entire application so that it covers both frontend and backend.