---
weight: 4
bookFlatSection: true
title: "Resources"
---

# Resources

Resources belong to Organizations - and they're names of various entities in the Organization. We use Unix-like paths to represent entities in an organization.

Let's see some examples.

Here's how you'd store some file paths.

- /files/legal/abc.doc
- /files/marketing/another.html

But anything can be a path. For example, if you wanted to control access to a feature, you could define a path as follows and check a user's permissions to it.

- /features/sellbitcoin
- /features/bulkbuy

## Create a Resource

```tpl
HTTP POST /orgs/{orgId}/resources

For example:
HTTP POST /orgs/example.com/resources
```

Payload:

```json
{
  "id": "/root/drives/c/home",
  "data": "data for /root/drives/c/home"
}
```

Response:

```json
{
  "data": {
    "id": "/root/drives/c/home",
    "data": "data for /root/drives/c/home",
    "createdAt": "2023-07-06T13:40:03.4217161Z",
    "orgId": "example.com"
  }
}
```

## Get Resources

```tpl
HTTP GET /orgs/{orgId}/resources

For example:
HTTP GET /orgs/example.com/resources
```

Response:

```json
{
  "data": [
    {
      "id": "/root/drives/c/home",
      "data": "data for /root/drives/c/home",
      "createdAt": "2023-07-06T13:40:03.421716Z",
      "orgId": "example.com"
    }
  ]
}
```

Pagination is done with the `from` and `limit` query parameters.

```tpl
HTTP GET /orgs/example.com/resources?from=10&limit=100
```

To get a single resource (as a list with one item), specific the Resource's path.

```tpl
HTTP GET /orgs/{orgId}/resources/{resourcePath}

# For example
HTTP GET /orgs/example.com/resources/root/drives/c/home
```

## Wildcard searches

A wildcard search allows you to search for all resources starting with a certain path. The wildcard character to use is a tilde(~).

```tpl
HTTP GET /orgs/{orgId}/resources/{basePath}/~

# For example
HTTP GET /orgs/example.com/resources/root/drives/~
```

Assume you have the following resources:

- /orgs/example.com/resources/root/drives/c/home
- /orgs/example.com/resources/root/drives/d/home

The request `HTTP GET /orgs/example.com/resources/root/drives/~` will fetch both since they start with `/root/drives/`.

## Update a Resource

```tpl
HTTP PUT /orgs/{orgId}/resources/{resourcePath}

For example:
HTTP PUT /orgs/example.com/resources/root/drives/c/home
```

Payload:

```json
{
  "data": "new data for /root/drives/c/home"
}
```

Response:

```json
{
  "id": "/root/drives/c/home",
  "data": "new data for /root/drives/c/home",
  "createdAt": "2023-06-30T13:39:29.182724Z",
  "orgId": "agilehead.com"
}
```

## DELETE a Resource

```tpl
HTTP DELETE /orgs/{orgId}/resources/{resourcePath}

For example:
HTTP DELETE /orgs/example.com/resources/root/drives/c/home
```

## Permissions

See the [Permissions API](../permissions).