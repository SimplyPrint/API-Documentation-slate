# Printer Groups

## Get groups

```shell
curl https://api.simplyprint.io/{id}/groups/Get \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Success response

```json
{
  "status": true,
  "message": null,
  "groups": [
    {
      "id": 1,
      "name": "Group 1"
    },
    {
      "id": 2,
      "name": "Group 2"
    }
  ]
}
```

`GET /{id}/groups/Get`

### Response

| Parameter       | Type    | Description                           |
|-----------------|---------|---------------------------------------|
| `status`        | boolean | `true` if the request was successful. |
| `message`       | string  | Error message if `status` is `false`. |
| `groups`        | array   | Array of printer groups.              |
| `groups[].id`   | integer | Unique identifier for the group.      |
| `groups[].name` | string  | Name of the group.                    |

## Create group

```shell
curl https://api.simplyprint.io/{id}/groups/Create \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "name": "New Group Name"
  }'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "id": 123
}
```

`POST /{id}/groups/Create`

### Request Body

| Parameter | Type   | Description                |
|-----------|--------|----------------------------|
| `name`    | string | The name of the new group. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
| `id`      | integer | Unique identifier for the new group.  |

## Update group

```shell
curl https://api.simplyprint.io/{id}/groups/Update?group=123 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "name": "Updated Group Name"
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/groups/Update`

### Request Parameters

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| `group`   | integer | The ID of the group to update. |

### Request Body

| Parameter | Type   | Description                 |
|-----------|--------|-----------------------------|
| `name`    | string | The new name for the group. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Delete group

```shell
curl https://api.simplyprint.io/{id}/groups/Delete?group=123 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/groups/Delete`

### Request Parameters

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| `group`   | integer | The ID of the group to delete. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Arrange groups

```shell
curl https://api.simplyprint.io/{id}/groups/Arrange?pid=1234&group=123 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "from": 1,
    "to": 2
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/groups/Arrange`

### Request Parameters

| Parameter | Type    | Description                       |
|-----------|---------|-----------------------------------|
| `pid`     | integer | The ID of the printer to arrange. |
| `group`   | integer | The ID of the group to move to.   |

### Request Body

| Parameter | Type    | Description                          |
|-----------|---------|--------------------------------------|
| `from`    | integer | The current position of the printer. |
| `to`      | integer | The new position of the printer.     |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
