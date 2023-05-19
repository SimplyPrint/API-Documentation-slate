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

This endpoint adds a file to the queue. The file can either be a file on the filesystem or an uploaded stl/3mf/obj/gcode/gco/nc/npg file.

**Note:** if you want to specify which printer/printer type/printer model the print job should be assigned, you can [edit the print job](#update-queue-item) after it has been added to the queue.

### Request

`POST /{id}/queue/AddItem`

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `filesystem` | string | no | The filesystem id of the file to add to the queue. |
| `amount` | integer | no | The amount of prints to add to the queue.<br>**Default: 1** |
| `group` | integer | no | If you have Queue Groups - ID of the group the item should be added to.<br>**Default: 0 - required if you have Queue Groups** |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `status` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `status` is false. |
| `created_id` | integer | The id of the created queue item |

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
        "left": 1
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

This endpoint gets the next item in the queue for the specified printer. The next item is the item that has the highest priority. The result will have skipped all items that do not meet the specified conditions.

### Request

`POST /{id}/queue/GetNextItems`

#### Request parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| p | integer[] | yes | Comma separated list of printer ids to get the next items for. |

#### Request body

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `settings` | object | no | Conditions that must be met for the next item. |
| `settings.filament` | boolean | no | Must have enough filament.<br>**Default: true** |
| `settings.filamentTemps` | boolean | no | Printer's filament temperature must match filament temperature of file.<br>**Default: true** |
| `settings.fit` | boolean | no | Print must fit printer's bed.<br>**Default: true** |
| `settings.gcodeAnalysis` | boolean | no | Must have gcode analysis.<br>**Default: true** |
| `settings.printerTemps` | boolean | no | File must have a max temperature that is lower than the printer's max temperature.<br>**Default: true** |
| `settings.tags` | boolean | no | Printer must match possible queue item tags (nozzle size, material data & custom tags).<br>**Default: true** |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `status` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `status` is false. |
| `queue` | object | The queue object. |
| `queue.total` | integer | The total amount of items in the queue. |
| `queue.printers` | integer[] | The printer ids that were requested. |
| `queue.matches` | array | The next items for each printer. |
| `queue.matches[].id` | integer | The id of the next item. Only present if `match` is true. |
| `queue.matches[].index` | integer | The index of the item in the queue. Only present if `match` is true. |
| `queue.matches[].printer` | integer | The id of the printer that the item is for. |
| `queue.matches[].match` | boolean | True if a match was found. |
| `queue.matches[].issues` | string[] | The issues that are present in the item. Can also have values if an item was matched but would have been catched by other settings. |
| `queue.matches[].missed` | integer | The amount of items that were skipped. |
| `queue.matches[].name` | string | The name of the item. Only present if `match` is true. |
| `queue.matches[].printed` | integer | The amount of completed prints of this item (from print queue). Only present if `match` is true. |
| `queue.matches[].left` | integer | The amount of prints left (from print queue). Only present if `match` is true. |

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
        "model": false,
        "printable": true,
        "left": 1,
        "printed": 0,
        "filesystem_id": "c00489ef361771ac098b5a60e6740757",
        "group": 0,
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
              "hex":"#4CAF50"
            }
          ],
          "custom": [1, 2, 3]
        }
      }
    ]
  }
}
```

<aside class="notice">
  This endpoint requires the <b>Pro</b> plan.
</aside>

This endpoint returns the queue for the specified or all printers.

### Request

`GET /{id}/queue/GetItems`

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `p` | integer | no | The printer id to get the queue for. If not specified, the queue for all printers will be returned. |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `status` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `status` is false. |
| `queue` | object | The queue object. |
| `queue.fits` | boolean | Whether any items in the queue can be printed on the printer. (due to space, temperature) |
| `queue.items` | array | An array of queue item objects. |
| `queue.items[].id` | integer | The queue item id. |
| `queue.items[].index` | integer | The queue item index. |
| `queue.items[].filename` | string | The queue item filename. |
| `queue.items[].model` | boolean | True if the queue item is a model. |
| `queue.items[].printable` | boolean | True if the queue is printable. |
| `queue.items[].left` | integer | The amount of prints left to print. |
| `queue.items[].printed` | integer | The amount of prints that have been printed. |
| `queue.items[].filesystem_id` | string/null | File id if print is from SimplyPrint filesystem. | 
| `queue.items[].group` | integer | Possible ID of Queue Group. |
| `queue.items[].for` | object | For which printers, models and groups this queue item is for. |
| `queue.items[].for.printers` | array | An array of printer ids. |
| `queue.items[].for.models` | array | An array of printer model ids. |
| `queue.items[].for.groups` | array | An array of group ids. |
| `queue.items[].analysis` | object | The analysis object. |
| `queue.items[].analysis.slicer` | string | The slicer used to slice the file. |
| `queue.items[].analysis.filament` | array | An array of filament lengths. |
| `queue.items[].analysis.estimate` | integer | The estimated print time in seconds. |
| `queue.items[].analysis.movement` | object | The movement object. |
| `queue.items[].analysis.temps` | object | The temperatures object. |
| `queue.items[].analysis.temps.tool` | object | Temperature for each tool (extruder). |
| `queue.items[].analysis.temps.bed` | integer | Temperature for the bed. |
| `queue.items[].analysis.modelSize` | object | The model size object. Represented as `x`, `y` and `z` values in millimeters. |
| `queue.items[].analysis.printArea` | object | The print area object. Represented as `maxX`, `minX`, `maxY`, `minY`, `maxZ` and `minZ` values in millimeters. |
| `queue.items[].analysis.minDeltaRadius` | float | Minimum radius for delta printers. |
| `queue.items[].analysis.v` | integer | The analysis version. |
| `queue.items[].user` | string | The user name of who added the queue item. |
| `queue.items[].user_id` | integer | The user id of who added the queue item. |
| `queue.items[].tags` | object|nullable | Tags for queue item; custom tags, static material data & nozzle size |
| `queue.done_items` | array | If `groups` GET is set, an array of done queue items, or ones where the last remaining item is being printed **includes all the same fields as queue items, with a few extra;**. |
| `queue.done_items[]....` |  | *Fields inherited from regular queue items*. |
| `queue.done_items[].size` | integer | Byte-size used by this item - 0 if the file is from the filesystem. |
| `queue.done_items[].ongoing` | boolean | If the item is currently ongoing. |
| `queue.done_items[].done` | UTC date|nullable | UTC date that the item was finished. |
| `queue.done_items[].expires` | UTC date|nullable | UTC date that the item expires and is removed from the platform. |

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

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `job` | integer | yes | The queue item id to update. |

#### Request body

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `for_groups` | array | no | An array of group ids to assign the queue item to. |
| `for_models` | array | no | An array of printer model ids to assign the queue item to. |
| `for_printers` | array | no | An array of printer ids to assign the queue item to. |
| `amount` | integer | no | The new amount to set. |
| `amount` | integer | no | Set amount of "printed". |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `status` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `status` is false. |

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

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `job` | integer | yes | The queue item id to delete. |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `status` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `status` is false. |

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

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `job` | integer | yes | The queue item id to move. |
| `from` | integer | yes | The current position of the queue item. |
| `to` | integer | yes | The new position of the queue item. |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `success` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `success` is false. |

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

| Required Permissions |
| -------------------- |
| `PRINT_QUEUE_REMOVE_ALL` |

This endpoint empties the queue.

### Request

`GET /{id}/queue/EmptyQueue`

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `group` | integer | no | ID of Queue Group to empty.<br>**Default: 0 - required if you have Queue Groups |

### Response

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `status` | boolean | True if the request was successful. |
| `message` | string | Success message or error message if `status` is false. |
