# Queue

## Add item to queue

```shell
curl https://api.simplyprint.io/{id}/queue/AddItem \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "filesystem": "1a077dd6296417fe75555bf806b68089",
  "amount": 5,
  "group": 0
}
```

> Success response

```json
{
  "status": true,
  "message": null,
  "created_id": 1337
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

<aside class="notice">
  You can only upload files through the API using <a href="#api-files">API Files</a>
</aside>

This endpoint adds a file to the queue. The file can either be a file on the filesystem or an uploaded
stl/3mf/obj/gcode/gco/nc/npg file.

**Note:** if you want to specify which printer/printer type/printer model the print job should be assigned, you
can [edit the print job](#update-queue-item) after it has been added to the queue.

### Request

`POST /{id}/queue/AddItem`

| Parameter       | Type    | Required | Description                                                                                                                                                                                                                |
|-----------------|---------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `filesystem`    | string  | no       | The [filesystem](#files) id of the file to add to the queue.                                                                                                                                                               |
| `amount`        | integer | no       | The amount of prints to add to the queue.<br>**Default: 1**                                                                                                                                                                |
| `group`         | integer | no       | If you have Queue Groups - ID of the group the item should be added to.<br>**Default: 0 - required if you have Queue Groups**                                                                                              |
| `fileId`        | string  | no       | Optional File ID from [API File](#api-files) - use this to add a file uploaded via the API.                                                                                                                                |
| `tags`          | object  | no       | Tags to assign. Only send [nozzle](#assign-nozzle-size-tag) body, [material](#assign-material-tag) body or [custom](#assign-custom-tag) body, without `type`, `id` or `edited`                                             |
| `for_printers`  | array   | no       | An array of printer ids to assign the queue item to.                                                                                                                                                                       |
| `for_models`    | array   | no       | An array of printer model ids to assign the queue item to.                                                                                                                                                                 |
| `for_groups`    | array   | no       | An array of group ids to assign the queue item to.                                                                                                                                                                         |
| `custom_fields` | array   | no       | An array with custom fields to assign to the queue item. Each custom field consists of `{customFieldId: string, value: <value>}` where the `<value>` is a [Custom Field Submission Value](#custom-field-submission-value). |

### Response

| Parameter    | Type    | Description                                            |
|--------------|---------|--------------------------------------------------------|
| `status`     | boolean | True if the request was successful.                    |
| `message`    | string  | Success message or error message if `status` is false. |
| `created_id` | integer | The id of the created queue item                       |

## Get next queue item

```shell
curl https://api.simplyprint.io/{id}/queue/GetNextItems?p=1234 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "settings": {
    "filament": false,
    "filamentTemps": false,
    "fit": true,
    "gcodeAnalysis": false,
    "printerTemps": false
  }
}
```

> Success response

```json
{
  "status": true,
  "message": null,
  "queue": {
    "total": 15,
    "printers": [
      385
    ],
    "matches": [
      {
        "id": 1212,
        "index": 5,
        "printer": 385,
        "match": true,
        "issues": [
          "size",
          "temps"
        ],
        "missed": 0,
        "name": "Benchy.gcode",
        "printed": 2,
        "left": 1,
        "customFields": [
          {
            "id": "student_id",
            "value": {
              "string": "1234567890"
            }
          }
        ]
      }
    ]
  }
}
```

> Failed response (Could not find any items that match the specified conditions)

```json
{
  "status": true,
  "message": null,
  "queue": {
    "total": 15,
    "printers": [
      385
    ],
    "matches": [
      {
        "printer": 385,
        "match": false,
        "issues": [
          "size",
          "temps"
        ],
        "missed": 4
      }
    ]
  }
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint gets the next item in the queue for the specified printer. The next item is the item that has the highest
priority. The result will have skipped all items that do not meet the specified conditions.

### Request

`POST /{id}/queue/GetNextItems`

#### Request parameters

| Parameter | Type      | Required | Description                                                    |
|-----------|-----------|----------|----------------------------------------------------------------|
| p         | integer[] | yes      | Comma separated list of printer ids to get the next items for. |

#### Request body

| Parameter                | Type    | Required | Description                                                                                                  |
|--------------------------|---------|----------|--------------------------------------------------------------------------------------------------------------|
| `settings`               | object  | no       | Conditions that must be met for the next item.                                                               |
| `settings.filament`      | boolean | no       | Must have enough filament.<br>**Default: true**                                                              |
| `settings.filamentTemps` | boolean | no       | Printer's filament temperature must match filament temperature of file.<br>**Default: true**                 |
| `settings.fit`           | boolean | no       | Print must fit printer's bed.<br>**Default: true**                                                           |
| `settings.gcodeAnalysis` | boolean | no       | Must have gcode analysis.<br>**Default: true**                                                               |
| `settings.printerTemps`  | boolean | no       | File must have a max temperature that is lower than the printer's max temperature.<br>**Default: true**      |
| `settings.tags`          | boolean | no       | Printer must match possible queue item tags (nozzle size, material data & custom tags).<br>**Default: true** |

### Response

| Parameter                 | Type      | Description                                                                                                                         |
|---------------------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------|
| `status`                  | boolean   | True if the request was successful.                                                                                                 |
| `message`                 | string    | Success message or error message if `status` is false.                                                                              |
| `queue`                   | object    | The queue object.                                                                                                                   |
| `queue.total`             | integer   | The total amount of items in the queue.                                                                                             |
| `queue.printers`          | integer[] | The printer ids that were requested.                                                                                                |
| `queue.matches`           | array     | The next items for each printer.                                                                                                    |
| `queue.matches[].id`      | integer   | The id of the next item. Only present if `match` is true.                                                                           |
| `queue.matches[].index`   | integer   | The index of the item in the queue. Only present if `match` is true.                                                                |
| `queue.matches[].printer` | integer   | The id of the printer that the item is for.                                                                                         |
| `queue.matches[].match`   | boolean   | True if a match was found.                                                                                                          |
| `queue.matches[].issues`  | string[]  | The issues that are present in the item. Can also have values if an item was matched but would have been catched by other settings. |
| `queue.matches[].missed`  | integer   | The amount of items that were skipped.                                                                                              |
| `queue.matches[].name`    | string    | The name of the item. Only present if `match` is true.                                                                              |
| `queue.matches[].printed` | integer   | The amount of completed prints of this item (from print queue). Only present if `match` is true.                                    |
| `queue.matches[].left`    | integer   | The amount of prints left (from print queue). Only present if `match` is true.                                                      |

## Get queue item

```shell
curl https://api.simplyprint.io/{id}/queue/GetItem?id=1234 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "queue": {
    "id": 51293,
    "index": 1,
    "filename": "benchy.gcode",
    "note": null,
    "model": false,
    "printable": true,
    "type": "printable",
    "zipPrintable": false,
    "zipNoModel": false,
    "left": 1,
    "printed": 0,
    "filesystem_id": "c00489ef361771ac098b5a60e6740757",
    "group": 123,
    "for": {
      "printers": [
        1234
      ],
      "models": [
        1234
      ],
      "groups": [
        1234
      ]
    },
    "analysis": {
      "slicer": "Simplify3D",
      "filament": [
        60
      ],
      "estimate": 240,
      "movement": {
        "mRelative": 0,
        "eRelative": 0
      },
      "temps": {
        "tool": {
          "T0": 210
        },
        "bed": 50,
        "pset": 1
      },
      "modelSize": {
        "x": 151,
        "y": 16,
        "z": 5
      },
      "printArea": {
        "maxX": 156.05,
        "minX": 5,
        "maxY": 157.86,
        "minY": 142.14,
        "maxZ": 5,
        "minZ": 0.2
      },
      "minDeltaRadius": 313.91,
      "v": 5
    },
    "tags": {
      "nozzle": 0.6,
      "material": [
        {
          "ext": 0,
          "type": 123,
          "color": "Green",
          "hex": "#4CAF50"
        }
      ],
      "custom": [
        1,
        2,
        3
      ]
    }
  }
}
```

<aside class="notice">
  This endpoint requires the <b>Pro</b> plan.
</aside>

This endpoint returns the queue item with the specified id.

### Request

`GET /{id}/queue/GetItem

| Parameter | Type    | Required | Description                           |
|-----------|---------|----------|---------------------------------------|
| `id`      | integer | yes      | The queue item id to get details for. |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |
| `item`    | object  | The queue item object.                                 |

## Get queue items

```shell
curl https://api.simplyprint.io/{id}/queue/GetItems?p=1234 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "queue": {
    "fits": true,
    "items": [
      {
        "id": 51293,
        "index": 1,
        "filename": "benchy.gcode",
        "note": null,
        "model": false,
        "printable": true,
        "left": 1,
        "printed": 0,
        "filesystem_id": "c00489ef361771ac098b5a60e6740757",
        "group": 123,
        "for": {
          "printers": [
            1234
          ],
          "models": [
            1234
          ],
          "groups": [
            1234
          ]
        },
        "analysis": {
          "slicer": "Simplify3D",
          "filament": [
            60
          ],
          "estimate": 240,
          "movement": {
            "mRelative": 0,
            "eRelative": 0
          },
          "temps": {
            "tool": {
              "T0": 210
            },
            "bed": 50,
            "pset": 1
          },
          "modelSize": {
            "x": 151,
            "y": 16,
            "z": 5
          },
          "printArea": {
            "maxX": 156.05,
            "minX": 5,
            "maxY": 157.86,
            "minY": 142.14,
            "maxZ": 5,
            "minZ": 0.2
          },
          "minDeltaRadius": 313.91,
          "v": 5
        },
        "user": "John Doe",
        "user_id": 1234,
        "tags": {
          "nozzle": 0.6,
          "material": [
            {
              "ext": 0,
              "type": 123,
              "color": "Green",
              "hex": "#4CAF50"
            }
          ],
          "custom": [
            1,
            2,
            3
          ]
        },
        "customFields": [
          {
            "id": "student_id",
            "value": {
              "string": "1234567890"
            }
          }
        ]
      }
    ]
  },
  "groups": [
    {
      "id": 123,
      "name": "Group 1",
      "virtual": false,
      "extensions": [
        "gcode",
        "gco",
        "stl"
      ],
      "sort_order": 0
    },
    ...
  ]
}
```

<aside class="notice">
  This endpoint requires the <b>Pro</b> plan.
</aside>

This endpoint returns the queue for the specified or all printers.

### Request

`GET /{id}/queue/GetItems`

| Parameter | Type    | Required | Description                                                                                         |
|-----------|---------|----------|-----------------------------------------------------------------------------------------------------|
| `p`       | integer | no       | The printer id to get the queue for. If not specified, the queue for all printers will be returned. |
| `groups`  | boolean | no       | Attaches a list of print queue groups to the response. Note: this argument does not take a value.   |

### Response

| Parameter                               | Type          | Description                                                                                                                                                                      |
|-----------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `status`                                | boolean       | True if the request was successful.                                                                                                                                              |
| `message`                               | string        | Success message or error message if `status` is false.                                                                                                                           |
| `queue`                                 | object        | The queue object.                                                                                                                                                                |
| `queue.fits`                            | boolean       | Whether any items in the queue can be printed on the printer. (due to space, temperature)                                                                                        |
| `queue.items`                           | array         | An array of queue item objects.                                                                                                                                                  |
| `queue.items[].id`                      | integer       | The queue item id.                                                                                                                                                               |
| `queue.items[].index`                   | integer       | The queue item index.                                                                                                                                                            |
| `queue.items[].filename`                | string        | The queue item filename.                                                                                                                                                         |
| `queue.items[].note`                    | string/null   | Optional note text.                                                                                                                                                              |
| `queue.items[].model`                   | boolean       | True if the queue item is a model.                                                                                                                                               |
| `queue.items[].printable`               | boolean       | True if the queue is printable.                                                                                                                                                  |
| `queue.items[].left`                    | integer       | The amount of prints left to print.                                                                                                                                              |
| `queue.items[].printed`                 | integer       | The amount of prints that have been printed.                                                                                                                                     |
| `queue.items[].filesystem_id`           | string/null   | File id if print is from SimplyPrint filesystem.                                                                                                                                 |
| `queue.items[].group`                   | integer       | Possible ID of Queue Group.                                                                                                                                                      |
| `queue.items[].for`                     | object        | For which printers, models and groups this queue item is for.                                                                                                                    |
| `queue.items[].for.printers`            | array         | An array of printer ids.                                                                                                                                                         |
| `queue.items[].for.models`              | array         | An array of printer model ids.                                                                                                                                                   |
| `queue.items[].for.groups`              | array         | An array of group ids.                                                                                                                                                           |
| `queue.items[].analysis`                | object        | The analysis object.                                                                                                                                                             |
| `queue.items[].analysis.slicer`         | string        | The slicer used to slice the file.                                                                                                                                               |
| `queue.items[].analysis.filament`       | array         | An array of filament lengths.                                                                                                                                                    |
| `queue.items[].analysis.estimate`       | integer       | The estimated print time in seconds.                                                                                                                                             |
| `queue.items[].analysis.movement`       | object        | The movement object.                                                                                                                                                             |
| `queue.items[].analysis.temps`          | object        | The temperatures object.                                                                                                                                                         |
| `queue.items[].analysis.temps.tool`     | object        | Temperature for each tool (extruder).                                                                                                                                            |
| `queue.items[].analysis.temps.bed`      | integer       | Temperature for the bed.                                                                                                                                                         |
| `queue.items[].analysis.modelSize`      | object        | The model size object. Represented as `x`, `y` and `z` values in millimeters.                                                                                                    |
| `queue.items[].analysis.printArea`      | object        | The print area object. Represented as `maxX`, `minX`, `maxY`, `minY`, `maxZ` and `minZ` values in millimeters.                                                                   |
| `queue.items[].analysis.minDeltaRadius` | float         | Minimum radius for delta printers.                                                                                                                                               |
| `queue.items[].analysis.v`              | integer       | The analysis version.                                                                                                                                                            |
| `queue.items[].user`                    | string        | The user name of who added the queue item.                                                                                                                                       |
| `queue.items[].user_id`                 | integer       | The user id of who added the queue item.                                                                                                                                         |
| `queue.items[].tags`                    | object/null   | Tags for queue item; custom tags, static material data & nozzle size                                                                                                             |
| `queue.done_items`                      | array         | If `groups` GET is set, an array of done queue items, or ones where the last remaining item is being printed **includes all the same fields as queue items, with a few extra;**. |
| `queue.done_items[]....`                |               | *Fields inherited from regular queue items*.                                                                                                                                     |
| `queue.done_items[].size`               | integer       | Byte-size used by this item - 0 if the file is from the filesystem.                                                                                                              |
| `queue.done_items[].ongoing`            | boolean       | If the item is currently ongoing.                                                                                                                                                |
| `queue.done_items[].done`               | UTC date/null | UTC date that the item was finished.                                                                                                                                             |
| `queue.done_items[].expires`            | UTC date/null | UTC date that the item expires and is removed from the platform.                                                                                                                 |
| `groups`                                | array         | If `groups` GET is set, an array of print queue groups.                                                                                                                          |
| `groups[].id`                           | integer       | The group id.                                                                                                                                                                    |
| `groups[].name`                         | string        | The group name.                                                                                                                                                                  |
| `groups[].virtual`                      | boolean       | Whether the group is a virtual queue group.                                                                                                                                      |
| `groups[].extensions`                   | array/null    | An array of file extensions that are allowed in the group.                                                                                                                       |
| `groups[].sort_order`                   | integer       | The sort index of the group.                                                                                                                                                     |

## Update queue item

```shell
curl https://api.simplyprint.io/{id}/queue/UpdateItem?job=1234 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "for_groups": [
    1234
  ],
  "for_models": [
    1234
  ],
  "for_printers": [
    1234
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

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint updates the queue item with the specified id.

### Request

`POST /{id}/queue/UpdateItem`

#### Query parameters

| Parameter | Type    | Required | Description                  |
|-----------|---------|----------|------------------------------|
| `job`     | integer | yes      | The queue item id to update. |

#### Request body

| Parameter      | Type    | Required | Description                                                |
|----------------|---------|----------|------------------------------------------------------------|
| `for_groups`   | array   | no       | An array of group ids to assign the queue item to.         |
| `for_models`   | array   | no       | An array of printer model ids to assign the queue item to. |
| `for_printers` | array   | no       | An array of printer ids to assign the queue item to.       |
| `amount`       | integer | no       | The new amount to set.                                     |
| `amount`       | integer | no       | Set amount of "printed".                                   |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Delete queue item

```shell
curl https://api.simplyprint.io/{id}/queue/DeleteItem?job=1234 \
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

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint deletes the queue item with the specified id.

### Request

`? /{id}/queue/DeleteItem`

| Parameter | Type    | Required | Description                  |
|-----------|---------|----------|------------------------------|
| `job`     | integer | yes      | The queue item id to delete. |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Change queue order

```shell
curl https://api.simplyprint.io/{id}/queue/SetOrder?job=1234&from=0&to=1 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "success": true,
  "message": null
}
```

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

This endpoint changes the order of the queue items by moving the queue item with the specified id.

### Request

`GET /{id}/queue/SetOrder`

| Parameter | Type    | Required | Description                             |
|-----------|---------|----------|-----------------------------------------|
| `job`     | integer | yes      | The queue item id to move.              |
| `from`    | integer | yes      | The current position of the queue item. |
| `to`      | integer | yes      | The new position of the queue item.     |

### Response

| Parameter | Type    | Description                                             |
|-----------|---------|---------------------------------------------------------|
| `success` | boolean | True if the request was successful.                     |
| `message` | string  | Success message or error message if `success` is false. |

## Empty queue

```shell
curl https://api.simplyprint.io/{id}/queue/EmptyQueue \
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

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required Permissions     |
|--------------------------|
| `PRINT_QUEUE_REMOVE_ALL` |

This endpoint empties the queue.

### Request

`GET /{id}/queue/EmptyQueue`

| Parameter | Type    | Required | Description                                                                     |
|-----------|---------|----------|---------------------------------------------------------------------------------|
| `group`   | integer | no       | ID of Queue Group to empty.<br>**Default: 0 - required if you have Queue Groups |

### Response

| Parameter | Type    | Description                                            |
|-----------|---------|--------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                    |
| `message` | string  | Success message or error message if `status` is false. |

## Revive item

```shell
curl https://api.simplyprint.io/{id}/queue/ReviveItem?job=1234 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/queue/ReviveItem`

### Request Parameters

| Parameter | Type    | Description                  |
|-----------|---------|------------------------------|
| `job`     | integer | The ID of the job to revive. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Get groups

<aside class="notice">
  Print Queue Groups require the <b>Pro</b> plan.
</aside>

```shell
curl https://api.simplyprint.io/{id}/queue/groups/Get \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "list": [
    {
      "id": 1,
      "name": "Queue Group 1",
      "virtual": false,
      "extensions": [
        "gcode",
        "bgcode"
      ],
      "sort_order": 1
    }
  ]
}
```

`GET /{id}/queue/groups/Get`

### Response

| Parameter           | Type    | Description                                                                      |
|---------------------|---------|----------------------------------------------------------------------------------|
| `status`            | boolean | `true` if the request was successful.                                            |
| `message`           | string  | Error message if `status` is `false`.                                            |
| `list`              | array   | Array of print queue groups.                                                     |
| `list[].id`         | integer | Unique identifier for the group.                                                 |
| `list[].name`       | string  | Name of the group.                                                               |
| `list[].virtual`    | boolean | Whether the group is a virtual queue group.                                      |
| `list[].extensions` | array   | An array of file extensions that are allowed in the group. (without punctuation) |
| `list[].sort_order` | integer | The sort order of the group.                                                     |
| `list[].for`        | object  | For which printers, models and groups this queue item is for.                    |

## Save group

```shell
curl https://api.simplyprint.io/{id}/queue/groups/Save \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "id": 123,
    "name": "New Queue Group",
    "accepted_extensions": ["gcode", "bgcode"],
    "virtual_only": false,
    "for_printers": "1,2,3",
    "for_models": "4,5,6",
    "for_groups": "7,8,9"
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/queue/groups/Save`

### Request Body

| Parameter             | Type    | Description                                                        |
|-----------------------|---------|--------------------------------------------------------------------|
| `id`                  | integer | The ID of the group to update (optional for creating a new group). |
| `name`                | string  | The name of the queue group.                                       |
| `accepted_extensions` | array   | List of accepted file extensions.                                  |
| `virtual_only`        | boolean | Whether the group is virtual only.                                 |
| `for_printers`        | string  | Comma-separated list of printer IDs.                               |
| `for_models`          | string  | Comma-separated list of printer model IDs.                         |
| `for_groups`          | string  | Comma-separated list of printer group IDs.                         |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Delete group

```shell
curl https://api.simplyprint.io/{id}/queue/groups/Delete?id=123 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/queue/groups/Delete`

### Request Parameters

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| `id`      | integer | The ID of the group to delete. |

### Request Body

| Parameter | Type    | Description                                                        |
|-----------|---------|--------------------------------------------------------------------|
| `move_to` | integer | The ID of the group to move items to. Defaults to any other group. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Set group order

```shell
curl https://api.simplyprint.io/{id}/queue/groups/SetOrder?queue_group=123 \
  -X POST \
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

`POST /{id}/queue/groups/SetOrder`

### Request Parameters

| Parameter     | Type    | Description                |
|---------------|---------|----------------------------|
| `queue_group` | integer | The ID of the queue group. |

### Request Body

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `to`      | integer | The new sorting order of the group. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
