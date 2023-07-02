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
	"id": "administrators",
	"data": "data for admins"
}
```

Response:

```json
{
	"data": {
		"id": "administrators",
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
HTTP GET /orgs/example.com/roles/administrators
```

You can specify multiple Role ids.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}

# For example
HTTP GET /orgs/example.com/roles/administrators,devops
```


## Update a Role

```tpl
HTTP PUT /orgs/{orgId}/roles/{roleId}

# For example:
HTTP PUT /orgs/example.com/roles/administrators
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
		"id": "administrators",
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
HTTP DELETE /orgs/example.com/roles/administrators
```

## Add a Custom Property

You can add custom string properties to an Role entity.

```tpl
HTTP PUT: /orgs/{orgId}/roles/{roleId}/properties/{propertyName}

For example:
HTTP PUT: /orgs/example.com/roles/administrators/properties/country
```

Payload:

```json
{
	"value": "India"
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

When properties are added to an Org, they are returned when the Org is fetched.

For example, `GET /orgs/example.com` will retrieve the following response. Note the added properties object.

```json
{
	"data": [
		{
			"id": "example.com",
			"createdAt": "2023-07-02T01:38:18.764642Z",
			"data": "data for example.com",
			"properties": {
				"country": "India"
			}
		}
	]
}
```

## Get the value of a property

Properties are usually read as a part of the fetching entity; Org in this case. But if you need to get a list of properties without fetching the entity you can use the following API.

```tpl
HTTP GET /orgs/{orgId}/properties/{propertyName}

# For example:
HTTP GET /orgs/example.com/properties/country
```

Response:

```json
{
	"data": [
		{
			"name": "country",
			"value": "India",
			"hidden": false,
			"createdAt": "2023-07-02T02:19:15.499711Z"
		}
	]
}
```


## Delete a Custom Property 

```tpl
HTTP DELETE: /orgs/{orgId}/properties/{propertyName}

# For example:
HTTP DELETE: /orgs/example.com/properties/country
```

## Creating Hidden Properties

When a property is hidden, it will not be included when the organization is fetched. To create a hidden property, add the hidden flag when creating the property.

To create a Hidden Property:

```tpl
HTTP PUT: /orgs/{orgId}/properties/{propertyName}

# For example:
HTTP PUT: /orgs/example.com/properties/revenue
```

Payload should include the `hidden` attribute:

```json
{
  "value": "2340000",
  "hidden": true
}
```

To include a hidden property when querying Orgs, it should be explicitly mentioned. Note that properties which aren't hidden are always included.

```tpl
HTTP GET /orgs?properties={propertyName}

For example:
HTTP GET /orgs?properties={revenue}
```

Response:

```json
{
	"data": [
		{
			"id": "example.com",
			"createdAt": "2023-07-02T01:38:18.764642Z",
			"data": "new data for example.com",
			"properties": {
				"country": "India",
				"revenue": "2340000"
			}
		}
	]
}
```

## Filtering by Property

Organizations may be filtered by the custom property.

```tpl
HTTP GET /orgs?properties.{propertyName}={propertyValue}

# For example, fetch orgs with country = India
HTTP GET /orgs?properties.country=India
```