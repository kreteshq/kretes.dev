---
pos: 9
title: Server-Side Templating
---

# Server-Side Templating

Kretes uses Boxwood as the default templating language server-side. Boxwood is very similar to the plain HTML. Here's a short snippet that shows its features in a nutshell:

```html
<h1>{title}</h1>
<for widget in widgets>
  <if widget.available>
    <strong>{widget.name}</strong>
  </if>
</for>
```