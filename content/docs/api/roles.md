---
weight: 1
bookFlatSection: true
title: "Roles"
---
# Roles

Roles belong to Organizations.

## Create a Role

```tpl
HTTP POST /orgs/{orgId}/roles

# For example:
HTTP POST /orgs/example.com/roles
```

Payload:

```json
{
	"id": "admins",
	"data": "data for admins"
}
```

Response:

```json
{
	"data": {
		"id": "admins",
		"data": "data for admins",
		"createdAt": "2023-07-02T08:31:59.7931788Z",
		"orgId": "example.com",
		"properties": {}
	}
}
```

## Get Roles

```tpl
HTTP GET /orgs/{orgId}/roles/

For example:
HTTP GET /orgs/example.com/roles/
```

Response:

```json
{
	"data": [
		{
			"id": "admins",
			"data": "data for admins",
			"createdAt": "2023-06-30T17:01:13.525215Z",
			"orgId": "agilehead.com",
			"properties": {
				"prop1": "value1"
			}
		},
		{
			"id": "devs",
			"data": "data for devs",
			"createdAt": "2023-07-01T11:56:04.039094Z",
			"orgId": "agilehead.com",
			"properties": {}
		}
	]
}
```

To get a single role (as a list with one item), specific the Role's id.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}

# For example
HTTP GET /orgs/example.com/roles/admins
```

You can specify multiple Role ids.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}

# For example
HTTP GET /orgs/example.com/roles/admins,devops
```


## Update a Role

```tpl
HTTP PUT /orgs/{orgId}/roles/{roleId}

# For example:
HTTP PUT /orgs/example.com/roles/admins
```

Payload:

```json
{
	"data": "new data for admins"
}
```

Response:

```json
{
	"data": {
		"id": "admins",
		"data": "new data for admins",
		"createdAt": "2023-07-02T08:31:59.793178Z",
		"orgId": "example.com",
		"properties": {}
	}
}
```

## Delete a Role

Deleting a role will also delete associated permissions.

```tpl
HTTP DELETE /orgs/{orgId}/roles/{roleId}

# For example:
HTTP DELETE /orgs/example.com/roles/admins
```

## Add a Custom Property

You can add custom string properties to an Role entity.

```tpl
HTTP PUT: /orgs/{orgId}/roles/{roleId}/properties/{propertyName}

For example:
HTTP PUT: /orgs/example.com/roles/admins/properties/privileged
```

Payload:

```json
{
	"value": "yes"
}
```

Response:

```json
{
	"data": {
		"name": "country",
		"value": "India",
		"hidden": false,
		"createdAt": "2023-07-02T02:19:15.499711Z"
	}
}
```

When properties are added to an Role, they are returned when the Role is fetched.

For example, `GET /orgs/example.com/roles/admins` will retrieve the following response. Note the added properties object.

```json
{
	"data": [
		{
			"id": "example.com",
			"createdAt": "2023-07-02T01:38:18.764642Z",
			"data": "data for admins",
			"properties": {
				"privileged": "yes"
			}
		}
	]
}
```

## Get the value of a property

Properties are usually read as a part of the fetching entity; Role in this case. But if you need to get a list of properties without fetching the entity you can use the following API.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}/properties/{propertyName}

# For example:
HTTP GET /orgs/example.com/properties/roles/roles/admins/privileged
```

Response:

```json
{
	"data": [
		{
			"roleId": "admins",
			"orgId": "example.com",
			"name": "privileged",
			"value": "yes",
			"hidden": false,
			"createdAt": "2023-07-02T12:31:01.978925Z"
		}
	]
}
```


## Delete a Custom Property 

```tpl
HTTP DELETE: /orgs/{orgId}/roles/{roleId}/properties/{propertyName}

# For example:
HTTP DELETE: /orgs/example.com/roles/admins/properties/privileged
```

## Creating Hidden Properties

When a property is hidden, it will not be included when the Role is fetched. To create a hidden property, add the hidden flag when creating the property.

To create a Hidden Property:

```tpl
HTTP PUT: /orgs/{orgId}/roles/{roleId}/properties/{propertyName}

# For example:
HTTP PUT: /orgs/example.com/roles/admins/properties/active
```

Payload should include the `hidden` attribute:

```json
{
  "active": "yes",
  "hidden": true
}
```

To include a hidden property when querying Roles, it should be explicitly mentioned. Note that properties which aren't hidden are always included.

```tpl
HTTP GET /orgs/{orgId}/roles?properties={propertyName}

For example:
HTTP GET /orgs/example.com/roles?properties=active
```

Response:

```json
{
	"data": [
		{
			"id": "admins",
			"data": "data for admins",
			"createdAt": "2023-07-02T12:30:55.981226Z",
			"orgId": "example.com",
			"properties": {
				"active": "yes",
				"privileged": "yes"
			}
		}
	]
}
```

## Filtering by Property

Roles may be filtered by the custom property.

```tpl
HTTP GET /orgs/{orgId}/roles?properties.{propertyName}={propertyValue}

# For example, fetch roles with active = yes
HTTP GET /orgs/example.com/roles?properties.active=yes
```

## Finds users in a Role

See a list of users who have a specific Role.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}/users

# For example, fetch roles with active = yes
HTTP GET /orgs/example.com/roles/admins/users
```

For more on assigning Roles to Users, see the [Users API](/docs/api/users).
