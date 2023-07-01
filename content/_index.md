---
title: Introduction
type: docs
---

# Introduction

{{< columns >}}
## What is Tankman?

Many SaaS apps need a way to store organizations, users, roles, permissions and to authorize access. Tankman manages all of these, and comes with a simple REST API. It is MIT licensed, [Open Source](https://github.com/lesser-app/tankman), downloadable as a binary, and uses PostgreSQL as the database.

Tankman is NOT an Identity and Authentication server. But it can play well with Open Source solutions like [Ory Kratos](https://github.com/ory/kratos), various OAuth based providers, or your own custom authentication mechanism.

## What can it do?

Here's a list:
- Save and retrieve Organizations, Roles, Users, Resources and Permissions
- Assign roles to users
- Assign permissions to users or roles
- Fetch permissions for a user for a resource

## It's an early Beta

Tankman is quite new, and you might find a few issues. We expect to iron out them out over the next few months and release the first version in Q4 2023. We'll need help - you're invited to [join us on GitHub](https://github.com/lesser-app/tankman).

## Why not umm... say Google Zanzibar?

Tankman is for startups who want a self-hosted, super lightweight solution. It provides basic primitives for authorization; which is more often than not what you need.

