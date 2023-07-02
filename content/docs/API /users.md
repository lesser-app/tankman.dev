---
weight: 3
bookFlatSection: true
title: "Users"
---
# Organizations

Organizations are at the root of the entity hierarchy, and should be the first thing you should create. You may create as many orgs are you need.

## Create an Organization

```tpl
HTTP POST /orgs
```

Payload:

```json
{
	"id": "example.com", // a unique id
	"data": "data for example.com" // any string
}
```

Response:

```json
{
	"data": {
		"id": "example.com",
		"createdAt": "2023-07-02T01:32:18.4557911Z",
		"data": "data for example.com",
		"properties": {}
	}
}
```

## Get Organizations

```tpl
HTTP GET /orgs
```

Response:

```json
{
	"data": [
		{
			"id": "example.com",
			"createdAt": "2023-07-02T01:38:18.764642Z",
			"data": "data for example.com",
			"properties": {}
		},
    {
			"id": "agilehead.com",
			"createdAt": "2023-07-01T01:38:18.764642Z",
			"data": "some other data",
			"properties": {}
		}
	]
}
```

Pagination is done with the `from` and `limit` query parameters.

```tpl
HTTP GET /orgs?from=10&limit=100
```

To get a single organization (as a list with one item), specific the Org's id.

```tpl
HTTP GET /orgs/{orgId}

# For example
HTTP GET /orgs/example.com
```

You can specify multiple Org ids.

```tpl
HTTP GET /orgs/{orgId1,orgId2}

# For example
HTTP GET /orgs/example.com,northwind
```

## Update an Organization

```tpl
HTTP PUT /orgs/{orgId}

# For example:
HTTP PUT /orgs/example.com
```

Payload:

```json
{
	"data": "new data for example.com"
}
```

Response:

```json
{
	"data": {
		"id": "example.com",
		"createdAt": "2023-07-02T01:38:18.764642Z",
		"data": "new data for example.com",
		"properties": {}
	}
}
```

## Delete an Organization

Deleting an organization will delete everything associated with it - resources, roles, users, permissions etc. This is indeed a very destructive operation.

```tpl
HTTP DELETE /orgs/{orgId}

# For example:
HTTP DELETE /orgs/example.com
```

Since it can cause a lot of damage, there's an CLI option to require a safetyKey for deleting organizations. 

You need to start tankman like this:

```sh
./tankman --safety-key $SAFETY_KEY

# For example:
./tankman --safety-key NOFOOTGUN
```

With the `--safety-key` option, HTTP DELETE should specify the safetyKey parameter as a query string.

```tpl
HTTP DELETE /orgs/{orgId}?safetyKey=$SAFETY_KEY

# For example
HTTP DELETE /orgs/example.com?safetyKey=NOFOOTGUN
```


## Add a Custom Property

You can add custom string properties to an Organization entity.

```tpl
HTTP PUT /orgs/{orgId}/properties/{propertyName}

For example:
HTTP PUT /orgs/example.com/properties/country
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
HTTP DELETE /orgs/{orgId}/properties/{propertyName}

# For example:
HTTP DELETE /orgs/example.com/properties/country
```

## Creating Hidden Properties

When a property is hidden, it will not be included when the organization is fetched. To create a hidden property, add the hidden flag when creating the property.

To create a Hidden Property:

```tpl
HTTP PUT /orgs/{orgId}/properties/{propertyName}

# For example:
HTTP PUT /orgs/example.com/properties/revenue
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