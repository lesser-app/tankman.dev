---
weight: 3
bookFlatSection: true
title: "Users"
---
# Users

Users belong to an organization. Users may belong to Roles, and can have Permissions to various Resources.

## Create a User

```tpl
HTTP POST /orgs/{orgId}/users

# For example:
HTTP POST /orgs/example.com/users
```

Payload:

```json
{
	"id": "user3",
	"identityProviderUserId": "jeswinpk@agilehead.com",
	"identityProvider": "Google",
	"data": "data for aser3"
}
```

Response:

```json
{
	"data": {
		"roleIds": [],
		"properties": {},
		"id": "user3",
		"data": "data for aser3",
		"identityProvider": "Google",
		"identityProviderUserId": "jeswinpk@agilehead.com",
		"createdAt": "2023-07-06T13:01:26.6594794Z",
		"orgId": "example.com"
	}
}
```

## Get Users

```tpl
HTTP GET /orgs/{orgId}/users/

For example:
HTTP GET /orgs/example.com/users/
```

Response:

```json
{
	"data": [
		{
			"roleIds": [],
			"properties": {},
			"id": "user3",
			"data": "data for aser3",
			"identityProvider": "Google",
			"identityProviderUserId": "jeswinpk@agilehead.com",
			"createdAt": "2023-07-06T13:01:26.659479Z",
			"orgId": "example.com"
		}
	]
}
```

To get a single user (as a list with one item), specify the user's id.

```tpl
HTTP GET /orgs/{orgId}/users/{userId}

# For example
HTTP GET /orgs/example.com/users/user3
```

You can specify multiple user ids.

```tpl
HTTP GET /orgs/{orgId}/users/{userId}

# For example
HTTP GET /orgs/example.com/users/user3,user5
```


## Update a user

```tpl
HTTP PUT /orgs/{orgId}/users/{userId}

# For example:
HTTP PUT /orgs/example.com/users/user3
```

Payload:

```json
{
	"identityProviderUserId": "jeswinpk@agilehead.com",
	"identityProvider": "Google",
	"data": "new data for user3"
}
```

Response:

```json
{
	"data": {
		"roleIds": [],
		"properties": {},
		"id": "user3",
		"data": "new data for user3",
		"identityProvider": "Google",
		"identityProviderUserId": "jeswinpk@agilehead.com",
		"createdAt": "2023-07-06T13:01:26.659479Z",
		"orgId": "example.com"
	}
}
```

## Delete a user

Deleting a user will also delete associated user permissions.

```tpl
HTTP DELETE /orgs/{orgId}/users/{userId}

# For example:
HTTP DELETE /orgs/example.com/users/user3
```

## Assiging Roles

```tpl
HTTP POST /orgs/{orgId}/users/{userId}/roles

# For example:
HTTP DELETE /orgs/example.com/users/user3/roles
```

Payload:

```json
{
	"roleId": "admins"
}
```

Response:

```json
{
	"data": {
		"userId": "user3",
		"roleId": "admins",
		"createdAt": "2023-07-01T11:57:54.0264874Z",
		"orgId": "agilehead.com"
	}
}
```

## Unassigning Roles

```tpl
HTTP DELETE /orgs/{orgId}/users/{userId}/roles/{roleId}

# For example:
HTTP DELETE /orgs/example.com/users/user3/roles/admins
```

## Finds users in a Role

See a list of users who have a specific Role.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}/users

# For example, fetch roles with active = yes
HTTP GET /orgs/example.com/roles/admins/users
```

## Add a Custom Property

You can add custom string properties to an user entity.

```tpl
HTTP PUT: /orgs/{orgId}/users/{userId}/properties/{propertyName}

For example:
HTTP PUT: /orgs/example.com/users/user3/properties/firstName
```

Payload:

```json
{
	"value": "Jeswin"
}
```

Response:

```json
{
	"data": {
		"name": "firstName",
		"value": "Jeswin",
		"hidden": false,
		"createdAt": "2023-07-06T13:07:41.8782012Z"
	}
}
```

When properties are added to an user, they are returned when the user is fetched.

For example, `GET /orgs/example.com/users/user3` will retrieve the following response. Note the added properties object.

```json
{
	"data": [
		{
			"roleIds": [],
			"properties": {
				"firstName": "Jeswin"
			},
			"id": "user3",
			"data": "new data for user3",
			"identityProvider": "Google",
			"identityProviderUserId": "jeswinpk@agilehead.com",
			"createdAt": "2023-07-06T13:01:26.659479Z",
			"orgId": "example.com"
		}
	]
}
```

## Get the value of a property

Properties are usually read as a part of the fetching entity; user in this case. But if you need to get a list of properties without fetching the entity you can use the following API.

```tpl
HTTP GET /orgs/{orgId}/users/{userId}/properties/{propertyName}

# For example:
HTTP GET /orgs/example.com/properties/users/users/user3/firstName
```

Response:

```json
{
	"data": [
		{
			"userId": "user3",
			"orgId": "example.com",
			"name": "firstName",
			"value": "Jeswin",
			"hidden": false,
			"createdAt": "2023-07-06T13:07:41.878201Z"
		}
	]
}
```


## Delete a Custom Property 

```tpl
HTTP DELETE: /orgs/{orgId}/users/{userId}/properties/{propertyName}

# For example:
HTTP DELETE: /orgs/example.com/users/user3/properties/firstName
```

## Creating Hidden Properties

When a property is hidden, it will not be included when the user is fetched. To create a hidden property, add the hidden flag when creating the property.

To create a Hidden Property:

```tpl
HTTP PUT: /orgs/{orgId}/users/{userId}/properties/{propertyName}

# For example:
HTTP PUT: /orgs/example.com/users/user3/properties/active
```

Payload should include the `hidden` attribute:

```json
{
  "active": "yes",
  "hidden": true
}
```

To include a hidden property when querying users, it should be explicitly mentioned. Note that properties which aren't hidden are always included.

```tpl
HTTP GET /orgs/{orgId}/users?properties={propertyName}

For example:
HTTP GET /orgs/example.com/users?properties=active
```

Response:

```json
{
	"data": [
		{
			"roleIds": [],
			"properties": {
				"active": "yes",
				"firstName": "Jeswin"
			},
			"id": "user3",
			"data": "new data for user3",
			"identityProvider": "Google",
			"identityProviderUserId": "jeswinpk@agilehead.com",
			"createdAt": "2023-07-06T13:01:26.659479Z",
			"orgId": "example.com"
		}
	]
}
```

## Filtering by Property

users may be filtered by the custom property.

```tpl
HTTP GET /orgs/{orgId}/users?properties.{propertyName}={propertyValue}

# For example, fetch users with active = yes
HTTP GET /orgs/example.com/users?properties.active=yes
```

Response:

```json
{
	"data": [
		{
			"roleIds": [],
			"properties": {
				"firstName": "Jeswin"
			},
			"id": "user3",
			"data": "new data for user3",
			"identityProvider": "Google",
			"identityProviderUserId": "jeswinpk@agilehead.com",
			"createdAt": "2023-07-06T13:01:26.659479Z",
			"orgId": "example.com"
		}
	]
}
```
