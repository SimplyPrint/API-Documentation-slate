# Tags

## Create or Update Custom Tag

```shell
curl -X POST https://api.simplyprint.io/{id}/tags/Create \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "name": "My Custom Tag",
  "badge": "success"
}
```

> Success response

```json
{
  "status": true,
  "message": null,
  "id": 2,
  "tags": [
    {
      "id": 1,
      "name": "A tag",
      "badge": "primary",
      "used_by": {
        "printers": 1,
        "printer_groups": 0,
        "files": 2,
        "queue_items": 4
      }
    },
    {
      "id": 2,
      "name": "My Custom Tag",
      "badge": "success"
    }
  ]
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permissions |
|----------------------|
| `EDIT_TAGS`          |

This endpoint creates or updates a custom tag.

### Request

`POST /{id}/tags/Create`

| Parameter | Type    | Required | Description                                                                                     |
|-----------|---------|----------|-------------------------------------------------------------------------------------------------|
| `id`      | integer | no       | ID of the tag to update. Leave empty to create a new tag.                                       |
| `name`    | string  | yes      | Name of the tag.                                                                                |
| `badge`   | string  | yes      | Color of the tag. Please refer to the [Colors](#colors) section for a list of available colors. |

### Response

| Parameter                       | Type    | Description                                                                            |
|---------------------------------|---------|----------------------------------------------------------------------------------------|
| `status`                        | boolean | True if the request was successful.                                                    |
| `message`                       | string  | Success message or error message if `status` is false.                                 |
| `id`                            | integer | ID of the created or updated tag.                                                      |
| `tags`                          | array   | Array of all tags.                                                                     |
| `tags.*.id`                     | integer | ID of the tag.                                                                         |
| `tags.*.name`                   | string  | Name of the tag.                                                                       |
| `tags.*.badge`                  | string  | [Color](#colors) of the tag.                                                           |
| `tags.*.used_by`                | object  | Only present if the tag is used on any printers, printer groups, files or queue items. |
| `tags.*.used_by.printers`       | integer | Number of printers the tag is used on.                                                 |
| `tags.*.used_by.printer_groups` | integer | Number of printer groups the tag is used on.                                           |
| `tags.*.used_by.files`          | integer | Number of files the tag is used on.                                                    |
| `tags.*.used_by.queue_items`    | integer | Number of queue items the tag is used on.                                              |

## Get Custom Tag(s)

> Get a single tag

```shell
curl https://api.simplyprint.io/{id}/tags/Get?id=2 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "tag": {
    "id": 2,
    "name": "My Custom Tag",
    "badge": "success"
  }
}
```

> Get all tags

```shell
curl https://api.simplyprint.io/{id}/tags/Get \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "tags": [
    {
      "id": 1,
      "name": "A tag",
      "badge": "primary",
      "used_by": {
        "printers": 1,
        "printer_groups": 0,
        "files": 2,
        "queue_items": 4
      }
    },
    {
      "id": 2,
      "name": "My Custom Tag",
      "badge": "success"
    }
  ]
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint gets a single tag or all tags.

### Request

`GET /{id}/tags/Get`

| Parameter | Type    | Required | Description                                        |
|-----------|---------|----------|----------------------------------------------------|
| `id`      | integer | no       | ID of the tag to get. Leave empty to get all tags. |

### Response

| Parameter     | Type    | Description                                                                            |
|---------------|---------|----------------------------------------------------------------------------------------|
| `status`      | boolean | True if the request was successful.                                                    |
| `message`     | string  | Success message or error message if `status` is false.                                 |
| `tag`         | object  | Only present if `id` is set.                                                           |
| `tag.id`      | integer | ID of the tag.                                                                         |
| `tag.name`    | string  | Name of the tag.                                                                       |
| `tag.badge`   | string  | [Color](#colors) of the tag.                                                           |
| `tag.used_by` | object  | Only present if the tag is used on any printers, printer groups, files or queue items. |
| `tags`        | array   | Only present if `id` is not set. Array of all tags.                                    |
| `tags.*`      | object  | Tag data like in the `tag` object above.                                               |

## Delete Custom Tag

```shell
curl https://api.simplyprint.io/{id}/tags/Delete?id=2 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "tags": [
    {
      "id": 1,
      "name": "A tag",
      "badge": "primary",
      "used_by": {
        "printers": 1,
        "printer_groups": 0,
        "files": 2,
        "queue_items": 4
      }
    }
  ]
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permissions |
|----------------------|
| `EDIT_TAGS`          |

This endpoint deletes a custom tag.

### Request

`GET /{id}/tags/Delete`

| Parameter | Type    | Required | Description              |
|-----------|---------|----------|--------------------------|
| `id`      | integer | yes      | ID of the tag to delete. |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |
| `tags`    | array   | Array of all tags.                                     |

## Assign Nozzle Size Tag

```shell
curl -X POST https://api.simplyprint.io/{id}/tags/Assign \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Request body

```json
{
  "type": 1,
  "id": 2315,
  "edited": "nozzle",
  "nozzle": 0.4
}
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

This endpoint assigns a nozzle size tag to a printer, printer group, file or queue item.

Please note that to assign a tag to a printer, you need the `EDIT_PRINTER` permission.

### Request

`POST /{id}/tags/Assign`

| Parameter | Type           | Required | Description                                                                                          |
|-----------|----------------|----------|------------------------------------------------------------------------------------------------------|
| `type`    | integer        | yes      | What to assign the tag to. `1` for printer, `2` for printer group, `3` for file, `4` for queue item. |
| `id`      | integer/string | yes      | ID of the printer, printer group, file or queue item to assign the tag to.                           |
| `edited`  | string         | yes      | Set to `nozzle` to assign a nozzle size tag.                                                         |
| `nozzle`  | float          | yes      | Nozzle size to assign in millimeters.                                                                |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Assign Material Tag

```shell
curl -X POST https://api.simplyprint.io/{id}/tags/Assign \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Request body

```json
{
  "type": 1,
  "id": 2315,
  "edited": "material",
  "material": [
    {
      "ext": 0,
      "type": 412,
      "hex": "#ff0000",
      "color": "Red"
    }
  ]
}
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

This endpoint assigns a material tag to a printer, printer group, file or queue item.

Please note that to assign a tag to a printer, you need the `EDIT_PRINTER` permission.

### Request

`POST /{id}/tags/Assign`

| Parameter          | Type           | Required | Description                                                                                          |
|--------------------|----------------|----------|------------------------------------------------------------------------------------------------------|
| `type`             | integer        | yes      | What to assign the tag to. `1` for printer, `2` for printer group, `3` for file, `4` for queue item. |
| `id`               | integer/string | yes      | ID of the printer, printer group, file or queue item to assign the tag to.                           |
| `edited`           | string         | yes      | Set to `material` to assign a material tag.                                                          |
| `material`         | array          | yes      | Array of materials to assign.                                                                        |
| `material.*.ext`   | integer        | yes      | Material extruder to assign the tag to. (zero-indexed)                                               |
| `material.*.type`  | integer        | yes      | Material type id to assign.                                                                          |
| `material.*.hex`   | string         | yes      | Material color hex code.                                                                             |
| `material.*.color` | string         | yes      | Material color name.                                                                                 |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Assign Custom Tag

```shell
curl -X POST https://api.simplyprint.io/{id}/tags/Assign \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Request body

```json
{
  "type": 1,
  "id": 2315,
  "edited": "custom",
  "tag_id": 1
}
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint assigns a custom tag to a printer, printer group, file or queue item.

Please note that to assign a tag to a printer, you need the `EDIT_PRINTER` permission.

### Request

`POST /{id}/tags/Assign`

| Parameter | Type           | Required | Description                                                                                          |
|-----------|----------------|----------|------------------------------------------------------------------------------------------------------|
| `type`    | integer        | yes      | What to assign the tag to. `1` for printer, `2` for printer group, `3` for file, `4` for queue item. |
| `id`      | integer/string | yes      | ID of the printer, printer group, file or queue item to assign the tag to.                           |
| `edited`  | string         | yes      | Set to `custom` to assign a custom tag.                                                              |
| `tag_id`  | integer        | yes      | ID of the custom tag to assign.                                                                      |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Detach Custom Tag

```shell
curl -X POST https://api.simplyprint.io/{id}/tags/Detach \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Request body

```json
{
  "type": 1,
  "id": 2315,
  "tag_id": 1
}
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint detaches a custom tag from a printer, printer group, file or queue item.

Please note that to detach a tag from a printer, you need the `EDIT_PRINTER` permission.

### Request

`POST /{id}/tags/Detach`

| Parameter | Type           | Required | Description                                                                                            |
|-----------|----------------|----------|--------------------------------------------------------------------------------------------------------|
| `type`    | integer        | yes      | What to detach the tag from. `1` for printer, `2` for printer group, `3` for file, `4` for queue item. |
| `id`      | integer/string | yes      | ID of the printer, printer group, file or queue item to detach the tag from.                           |
| `tag_id`  | integer        | yes      | ID of the custom tag to detach.                                                                        |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Colors

| Color      | Name        | Preview                                                                                          |
|------------|-------------|--------------------------------------------------------------------------------------------------|
| Blue       | `primary`   | <span class="badge-pill" style="background-color: #4285f4;">Primary</span>                       |
| Purple     | `secondary` | <span class="badge-pill" style="background-color: #a6c;">Secondary</span>                        |
| Green      | `success`   | <span class="badge-pill" style="background-color: #00c851;">Success</span>                       |
| Red        | `danger`    | <span class="badge-pill" style="background-color: #ff3547;">Danger</span>                        |
| Yellow     | `warning`   | <span class="badge-pill" style="background-color: #fb3;">Warning</span>                          |
| Turquoise  | `info`      | <span class="badge-pill" style="background-color: #33b5e5;">Info</span>                          |
| Light grey | `light`     | <span class="badge-pill" style="background-color: #e0e0e0; color: #000 !important;">Light</span> |
| Dark       | `dark`      | <span class="badge-pill" style="background-color: #212121;">Dark</span>                          |
