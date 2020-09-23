---
pos: 1
title: Writing a Middleware
---

# Writing a Middleware

Middlewares are functions that execute in the request-response lifecycle. Such functions have access to the request and response for routes to which they are attached. They can change both the incoming and outgoing data by modifying the request and the response respectively. This functionality allows to group common tasks into a specific workflow e.g. validating the incoming data.
