---
title: Introduction
type: docs
---

# Introduction

{{< columns >}}
## What can Tankman do?

![Tankman](tankman-ai.jpg)

Tankman is a service which offers a REST API to do the following:

- Manage organizations, roles, users, resources and permissions
- Assign roles to users
- Assign user permissions and role permissions to various resources
- Attach custom properties to Orgs, Roles, Users and query them

It's important to note that Tankman does not function as an Identity and Authentication server. However, it can work well with popular open-source identity solutions such as [Ory Kratos](https://github.com/ory/kratos), various OAuth-based providers, or even your own custom authentication mechanism.

Tankman is available on GitHub under the MIT license. You can download it as a binary for your platform; and it utilizes PostgreSQL as its underlying database.

## Why should I use Tankman over Auth0, Google Zanzibar etc?

Tankman is for people who prefer a self-hosted, lightweight solution. It provides essential primitives for authorization, which are often sufficient for most use cases.

It is worth noting that the other alternatives mentioned are undoubtedly more capable and encompass a broader range of features. If you find yourself in need of those advanced features, it would be more suitable for you to opt for one of those alternatives.

## It's an early Beta

Tankman is relatively new, so you might encounter a few issues. We expect to iron them out over the next few months and plan to release the first version in Q4 2023. We welcome your help and invite you to [join us on GitHub](https://github.com/lesser-app/tankman).

