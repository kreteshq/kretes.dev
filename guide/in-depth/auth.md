---
pos: 8
title: Auth
---
# Authentication & Authorization

Kretes provides authentication and authorization out of the box. Usually even the simplest applications require those functionalities; that is the reason why it is a built-in feature in Kretes.

## Authentication

Kretes has two authentication helper functions: `register` and `login`.

::: alert Argon2 as the Hashing Function
bcrypt lacks memory hardness while in scrypt both, memory hardness and iteration count are tied to a single cost factor. Argon2 won the Password Hashing Competition in 2015. It is build around AES ciphers, is resistant to ranking tradeoff attacks.
:::

## Authorization