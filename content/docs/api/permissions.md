---
weight: 5
bookFlatSection: true
title: "Permissions"
---
# Permissions

Permissions are entities which determine whether a Role or a User has access to a Resource. Permissions also have an "action" property which specifies what the Role or User is allowed to do with the Resource.

## Creating a Role Permission

```tpl
HTTP POST /orgs/{orgId}/roles/{roleId}/permissions

For example:
HTTP POST /orgs/example.com/roles/admins/permissions
```

Payload:

```json
{
	"resourceId": "/root/drives/c/home",
	"roleId": "admins",
	"action": "write"
}
```

Response:

```json
{
	"roleId": "admins",
	"resourceId": "/root/drives/c/home",
	"action": "write",
	"createdAt": "2023-06-30T14:04:17.919562Z",
	"orgId": "agilehead.com"
}
```

The example above specifies that the role "admins" can "write" to the resource "/root/drives/c/home". Note that the resource should already exist. See the [Resources API](../resources) to learn how to create a resource.


## Creating a User Permission

```tpl
HTTP POST /orgs/{orgId}/users/{userId}/permissions

For example:
HTTP POST /orgs/example.com/users/user3/permissions
```

Payload:

```json
{
	"resourceId": "/root/drives/c/home",
	"userId": "user3",
	"action": "read"
}
```

Response:

```json
{
	"userId": "admins",
	"resourceId": "/root/drives/c/home",
	"action": "write",
	"createdAt": "2023-06-30T14:04:17.919562Z",
	"orgId": "agilehead.com"
}
```

The example above specifies that the user "user3" can "read" the resource "/root/drives/c/home".

## Getting Effective Permissions for a User

Effective Permissions for a user is the combined list of all user permissions defined for the specifc user, and role permissions for the roles in which the user is a member.

For example, if the user "john" is a member of roles "admins" and "devops", effective permissions to a resource is the combined list of john's permissions to the specific resource, the role "admins" permissions to the resource, and the role "devops" permissions to the resource.

```tpl
HTTP GET /orgs/{orgId}/users/{userId}/effective-permissions/{action}/{resourceId}
For example:
HTTP GET /orgs/example.com/users/user3/effective-permissions/write/root/drives/c/home
```

Response:

```json
[
	{
		"roleId": "admins",
		"resourceId": "/root/drives/c/home",
		"action": "write",
		"createdAt": "2023-06-30T14:04:17.919562Z",
		"orgId": "agilehead.com"
	}
]
```


## Wildcard actions

You can specify a wildcard (tilde "~", by default) to get all permissions irrespective of action.

For example, the following will fetch read and write actions:

```tpl
HTTP GET /orgs/example.com/users/user3/effective-permissions/~/root/drives/c/home
```

Response:

```json
[
	{
		"userId": "user3",
		"resourceId": "/root/drives/c/home",
		"action": "read",
		"createdAt": "2023-06-30T14:04:01.837162Z",
		"orgId": "agilehead.com"
	},
	{
		"roleId": "admins",
		"resourceId": "/root/drives/c/home",
		"action": "write",
		"createdAt": "2023-06-30T14:04:17.919562Z",
		"orgId": "agilehead.com"
	}
]
```

## Wildcard Resource Paths

You can specify a wildcard in the resource path as well.

For example, the following will return all resources starting with "/root/drives". Note that we've used a wild card action as well.

```tpl
HTTP GET /orgs/example.com/users/user3/effective-permissions/~/root/drives/~
```

## Getting just Role Permissions

You can get permissions for a role to a resource thus.

```tpl
HTTP GET /orgs/{orgId}/roles/{roleId}/permissions

For example:
HTTP GET /orgs/example.com/roles/admins/permissions
```

Response:

```json
{
	"data": {
		"roleId": "admins",
		"resourceId": "/root/drives/c/home",
		"action": "write",
		"createdAt": "2023-07-06T14:35:31.3918132Z",
		"orgId": "example.com"
	}
}
```

## Getting just User Permissions

You can get permissions for a user to a resource thus.

```tpl
HTTP GET /orgs/{orgId}/users/{userId}/permissions

For example:
HTTP GET /orgs/example.com/users/user3/permissions
```

Response:

```json
{
	"data": {
		"userId": "user3",
		"resourceId": "/root/drives/c/home",
		"action": "read",
		"createdAt": "2023-07-06T14:35:31.3918132Z",
		"orgId": "example.com"
	}
}
```

## Deleting Role Permissions

You can delete permissions for a role to a resource thus.

```tpl
HTTP DELETE /orgs/{orgId}/roles/{roleId}/permissions/{action}/{resourceId}

For example:
HTTP DELETE /orgs/example.com/roles/admins/permissions/read/root/drives/c/home
```

Response:

```json
{
	"data": {
		"roleId": "admins",
		"resourceId": "/root/drives/c/home",
		"action": "write",
		"createdAt": "2023-07-06T14:35:31.3918132Z",
		"orgId": "example.com"
	}
}
```

## Deleting User Permissions

You can delete permissions for a user to a resource thus.

```tpl
HTTP DELETE /orgs/{orgId}/users/{userId}/permissions/{action}/{resourceId}

For example:
HTTP DELETE /orgs/example.com/users/admins/permissions/read/root/drives/c/home
```

Response:

```json
{
	"data": {
		"userId": "admins",
		"resourceId": "/root/drives/c/home",
		"action": "write",
		"createdAt": "2023-07-06T14:35:31.3918132Z",
		"orgId": "example.com"
	}
}
```