# Printers

## Get printer info

```shell
curl https://api.simplyprint.io/{id}/printers/Get \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "page": 1,
  "page_size": 10,
  "search": "Printer 1"
}
```

> Success response

```json
{
  "status": true,
  "message": null,
  "page_amount": 1,
  "data": [
    {
      "id": 385,
      "sort_order": 6,
      "printer": {
        "name": "Mini Printer 1",
        "state": "printing",
        "group": 99,
        "position": 6,
        "api": "OctoPrint",
        "ui": "OctoPrint",
        "ip": "10.78.16.46",
        "public": 1,
        "machine": "Raspberry Pi 4 Model B Rev 1.2",
        "online": true,
        "region": "eu-west-2",
        "firmware": "Virtual Marlin",
        "spVersion": "4.0.5",
        "temps": {
          "ambient": 22,
          "current": {
            "tool": [
              210
            ],
            "bed": null
          },
          "target": {
            "tool": [
              210
            ],
            "bed": null
          }
        },
        "hasPSU": 1,
        "psuOn": true,
        "hasFilSensor": true,
        "filSensor": true,
        "filamentRetraction": 250,
        "model": {
          "id": 91,
          "name": "Fabrikator Mini",
          "brand": "Turnigy",
          "bedSize": [
            80,
            80
          ],
          "bedType": "square",
          "maxHeight": 80,
          "image": "https:\/\/cdn.simplyprint.io\/i\/printer_types\/turnigy\/fabrikator_mini\/silhouette_sm.png?cacheid=5fe9e77f49198",
          "hasHeatedBed": false,
          "extruders": 1,
          "extruderSettings": null,
          "maxToolTemp": 240,
          "maxBedTemp": 0,
          "filamentWidth": 1.75,
          "nozzleDia": 0.4,
          "axes": {
            "e": {
              "inverted": false,
              "speed": 5
            },
            "x": {
              "inverted": false,
              "speed": 100
            },
            "y": {
              "inverted": false,
              "speed": 100
            },
            "z": {
              "inverted": false,
              "speed": 3.5
            }
          },
          "screwOffset": 35,
          "filamentRetraction": 250,
          "customBoundingBox": false,
          "extrudeAbs": 0,
          "originCenter": 0,
          "bedBelt": 0,
          "fwRetract": 0,
          "extrusionType": 1,
          "noControl": 0
        },
        "hasCam": 1,
        "hasQueue": {
          "items": 6,
          "fits": true
        },
        "health": {
          "usage": 24,
          "temp": 61,
          "memory": 19
        },
        "unsupported": 0,
        "latency": null,
        "outOfOrder": 0
      },
      "filament": null,
      "job": {
        "id": 552252,
        "uid": "da69d2a4-e07e-48ff-128a-f88fab1b8f20",
        "state": "printing",
        "file": "Benchy.15mm_PLA_MK3S_7h40m.gcode",
        "percentage": 50,
        "time": 12627,
        "canPreview": true,
        "layer": null
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
    },
    ...
  ]
}
```

This endpoint returns a list of printers based on the given parameters.

### Request

`POST /{id}/printers/Get`

#### Query parameters

| Parameter | Type    | Required | Description                                                       |
|-----------|---------|----------|-------------------------------------------------------------------|
| `pid`     | integer | no       | Optional printer ID if you want to get info for a single printer. |

#### Request body

| Parameter   | Type    | Required | Description                                                     |
|-------------|---------|----------|-----------------------------------------------------------------|
| `page`      | integer | no       | Page number to get. Leave empty for page 1.                     |
| `page_size` | integer | no       | Number of printers per page. (Between 1 and 100)<br>Default: 10 |
| `search`    | string  | no       | Search string to filter printers by.                            |

### Response

Note that `data` will be an object if `pid` is specified, otherwise it will be an array.

| Parameter                           | Type            | Description                                                                      |
|-------------------------------------|-----------------|----------------------------------------------------------------------------------|
| `status`                            | boolean         | True if the request was successful.                                              |
| `message`                           | string          | Error message if `status` is false.                                              |
| `data`                              | object or array | Printer object(s).                                                               |
| `data.*.id`                         | integer         | Printer ID.                                                                      |
| `data.*.sort_order`                 | integer         | The printer's sort index.                                                        |
| `data.*.printer`                    | object          | Printer object.                                                                  |
| `data.*.printer.name`               | string          | Printer's name.                                                                  |
| `data.*.printer.state`              | string          | Printer's state.                                                                 |
| `data.*.printer.group`              | integer         | Printer's group ID.                                                              |
| `data.*.printer.position`           | integer         | Printer's position in the group.                                                 |
| `data.*.printer.api`                | string          | Printer's API type.                                                              |
| `data.*.printer.ui`                 | string          | Printer's UI type.                                                               |
| `data.*.printer.ip`                 | string          | Printer's local IP address.                                                      |
| `data.*.printer.public`             | boolean         | Whether the printer is shown on the company hub.                                 |
| `data.*.printer.machine`            | string          | Printer's machine type.                                                          |
| `data.*.printer.online`             | boolean         | Whether the printer is online.                                                   |
| `data.*.printer.region`             | string          | What region the printer is online in.                                            |
| `data.*.printer.firmware`           | string          | Printer's firmware version.                                                      |
| `data.*.printer.spVersion`          | string          | Printer's SimplyPrint version.                                                   |
| `data.*.printer.temps`              | object          | Printer's current temperatures.                                                  |
| `data.*.printer.hasPSU`             | boolean         | Whether the printer has a SimplyPrint connected PSU.                             |
| `data.*.printer.psuOn`              | boolean         | Whether the printer's PSU is on.                                                 |
| `data.*.printer.hasFilSensor`       | boolean         | Whether the printer has a filament sensor.                                       |
| `data.*.printer.filamentRetraction` | integer         | Printer's filament retraction distance.                                          |
| `data.*.printer.model`              | string          | Printer's model.                                                                 |
| `data.*.printer.hasCam`             | boolean         | Whether the printer has a camera.                                                |
| `data.*.printer.hasQueue`           | object          | Data about the printer's queue. Null if the printer doesn't have a queue.        |
| `data.*.printer.hasQueue.items`     | integer         | Number of items in the printer's queue.                                          |
| `data.*.printer.hasQueue.fits`      | boolean         | Whether the printer can physically fit any items in its queue.                   |
| `data.*.printer.health`             | object          | Printer's health data. (CPU usage, temperature, memory usage)                    |
| `data.*.printer.unsupported`        | boolean         | Whether the printer is unsupported.                                              |
| `data.*.printer.latency`            | integer         | Printer's latency.                                                               |
| `data.*.printer.outOfOrder`         | boolean         | Whether the printer is out of order.                                             |
| `data.*.filament`                   | object          | Printer's filament data.                                                         |
| `data.*.job`                        | object          | Printer's current job data. See [Get Print Jobs](#get-print-jobs) for more info. |
| `data.*.tags`                       | object/null     | Tags for printer; custom tags, static material data & nozzle size                |

## Start print / create job

```shell
curl https://api.simplyprint.io/{id}/printers/actions/CreateJob?pid=1234&filesystem=291 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "pid": [
    1234,
    1235
  ],
  "filesystem": "196a1dd0b93a66c19192a39fa4c16e71"
}
```

> Success response

```json
{
  "status": true,
  "message": null,
  "files": [
    {
      "name": "Benchy.gcode",
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
      "printers": [
        1234,
        1235
      ],
      "queued": false,
      "cost": [
        {
          "estimate": false,
          "total_cost": 1006.76,
          "lines": [
            {
              "id": 1,
              "label": "HIPS Material usage",
              "cost": 0.05
            },
            {
              "id": 2,
              "label": "Material markup",
              "cost": 0.03
            },
            {
              "id": 3,
              "label": "Machine run time cost",
              "cost": 6.67
            },
            {
              "id": 4,
              "label": "Energy cost",
              "cost": 0.01
            },
            {
              "id": 5,
              "label": "Labor cost",
              "cost": 1000
            }
          ]
        }
      ]
    }
  ],
  "jobIds": [
    495462,
    495463
  ]
}
```

<aside class="notice">
  You can only upload files through the API using <a href="#api-files">API Files</a>
</aside>

This endpoint can be used to create a print job for one or more printers. The printers have to be in the `operational`
state.

### Request

`POST /{id}/printers/actions/CreateJob`

To start a print job you must either specify a `filesystem` ID, a `queue_file` ID file or set `next_queue_item` to true.

| Parameter         | Type                 | Required | Description                                                                                                                                                                                                               |
|-------------------|----------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `pid`             | integer or integer[] | yes      | The ID(s) of the printer to create the job for.                                                                                                                                                                           |
| `filesystem`      | string               | no       | The filesystem ID of the file to print.                                                                                                                                                                                   |
| `queue_file`      | integer              | no       | The queue ID of the queue item to print.                                                                                                                                                                                  |
| `multi_queue`     | object               | no       | A map of queue item ID to an array of printer IDs. For example `{638: [12645, 12646]}`.                                                                                                                                   |
| `next_queue_item` | boolean              | no       | If true, the next queue item will be printed.<br>**This requires the Print Farm plan**                                                                                                                                    |
| `file_id`         | string               | no       | File ID from [API Files](#api-files) - used to start a file without adding it as a queue item or user file.                                                                                                               |
| `mms_map`         | object               | no       | A map of printer ids to a map of the extruder specified in the gcode, and the printer extruder index.                                                                                                                     |
| `custom_fields`   | array                | no       | An array with custom fields to assign to the print job. Each custom field consists of `{customFieldId: string, value: <value>}` where the `<value>` is a [Custom Field Submission Value](#custom-field-submission-value). |

#### Extra settings for `next_queue_item`

You can specify these parameters if `next_queue_item` is `true`. Note that you can specify more/all of the below
parameters.

| Parameter               | Type    | Required | Description                                                                                                                             |
|-------------------------|---------|----------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `analysis_strict`       | boolean | no       | Match the next item that has a valid gcode analysis.<br>**Defaults to true**                                                            |
| `fit_strict`            | boolean | no       | Match the next item if it fits on the printers print area.<br>**Defaults to true**                                                      |
| `temps_strict`          | boolean | no       | Match the next item where the printer can reach the temperatures specified in the gcode.<br>**Defaults to true**                        |
| `filament_strict`       | boolean | no       | Match the next item that was sliced for the same filament type that the printer is assigned to in SimplyPrint.<br>**Defaults to false** |
| `filament_temps_strict` | boolean | no       | Match the next item that has the same filament temperatures as the printer has in SimplyPrint.<br>**Defaults to false**                 |

### Response

| Field              | Type      | Description                                                                                             |
|--------------------|-----------|---------------------------------------------------------------------------------------------------------|
| `status`           | boolean   | True if the request was successful.                                                                     |
| `message`          | string    | Error message if `status` is false.                                                                     |
| `files`            | array     | Array of started print job objects.                                                                     |
| `files[].name`     | string    | The name of the file.                                                                                   |
| `files[].analysis` | object    | The analysis of the file. This has been documented in the [Get queue items endpoint](#get-queue-items). |
| `files[].printers` | integer[] | The IDs of the printers that the print job was started on.                                              |
| `files[].queued`   | boolean   | Whether the print job was from print queue.                                                             |
| `files[].cost`     | object[]  | nullable                                                                                                | Potential calculated cost of job. Potential of multiple costs if job is created for different printers with different material types assigned. |
| `jobIds`           | integer[] | The IDs of the print jobs that were started.                                                            |

## Pause print job

```shell
curl https://api.simplyprint.io/{id}/printers/actions/Pause?pid=1234 \
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

This endpoint can be used to pause one or multiple print jobs. The printers have to be in the `PRINTING` state.

### Request

`POST /{id}/printers/actions/Pause`

#### Query parameters

| Parameter | Type                 | Required | Description                                                                                                                            |
|-----------|----------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------|
| `pid`     | integer or integer[] | yes      | The ID(s) of the printer to pause. Pause multiple printers by comma separating printer ids.<br>**Printer must be in `PRINTING` state** |

#### Response

| Field     | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Resume print job

```shell
curl https://api.simplyprint.io/{id}/printers/actions/Resume?pid=1234 \
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

### Request

`POST /{id}/printers/actions/Resume`

#### Query parameters

| Parameter | Type                 | Required | Description                                                                                                                                    |
|-----------|----------------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `pid`     | integer or integer[] | yes      | The ID(s) of the printer to resume. Resume multiple printers by comma separating printer ids.<br>**Printer must be in `PRINTER_PAUSED` state** |

#### Response

| Field     | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Cancel print job

```shell
curl https://api.simplyprint.io/{id}/printers/actions/Cancel?pid=1234 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "reason": 3,
  "comment": "Cancel comment"
}
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

| Required permission    | Description                                                                                 |
|------------------------|---------------------------------------------------------------------------------------------|
| `cancel_others_prints` | Need permission to cancel other users' prints if the print job was started by another user. |

This endpoint can be used to cancel one or multiple print jobs. The printers have to be in the `PRINTING`, `PAUSED` or
`PAUSING` state.

### Request

`POST /{id}/printers/actions/Cancel`

#### Query parameters

| Parameter | Type                 | Required | Description                                                                                                                                                     |
|-----------|----------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `pid`     | integer or integer[] | yes      | The ID(s) of the printer to cancel. Cancel multiple printers by comma separating printer ids.<br>**Printer must be in `PRINTING`, `PAUSED` or `PAUSING` state** |

#### Request body

| Field     | Type    | Required | Description                                                                                                                                                                |
|-----------|---------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `reason`  | integer | no       | The reason for cancelling the print job. See [Cancel reasons](#cancel-reasons). Depending on the `require_cancel_reason` organization setting, this field may be required. |
| `comment` | string  | no       | A comment for the cancel reason. Depending on the `require_comment` organization setting, this field may be required.<br>**Max length: 500 characters**                    |

### Response

| Field     | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Clear print bed

```shell
curl https://api.simplyprint.io/{id}/printers/actions/ClearBed?pid=1234 \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "success": true,
  "rating": 4
}
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

This endpoint can be used to clear the print bed of a printer.

### Request

`POST /{id}/printers/actions/ClearBed`

#### Query parameters

| Parameter | Type    | Required | Description                                                                                                                    |
|-----------|---------|----------|--------------------------------------------------------------------------------------------------------------------------------|
| `pid`     | integer | yes      | The ID(s) of the printer to clear, comma separated. These printers have to be in either the `operational` or `offline` states. |

#### Request body

| Field     | Type    | Required | Description                                                                          |
|-----------|---------|----------|--------------------------------------------------------------------------------------|
| `success` | boolean | no       | True if the print was successful.<br>**Default: false**                              |
| `rating`  | integer | no       | The rating of the print. Don't send this field if you do not want to rate the print. |

### Response

| Field     | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Delete / disconnect printer

```shell
curl https://api.simplyprint.io/{id}/printers/Delete?pid=1234&just_connection=1 \
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

| Required permissions |
|----------------------|
| `printer_edit`       |

This endpoint can be used to delete a printer from the database, or to disconnect a pi from a printer. This is useful if
you want to change the printer that is connected to a pi.

### Request

`GET /{id}/printers/Delete`

| Parameter         | Type    | Required | Description                                                                                                                |
|-------------------|---------|----------|----------------------------------------------------------------------------------------------------------------------------|
| `pid`             | integer | yes      | The ID of the printer to delete.                                                                                           |
| `just_connection` | integer | no       | If set to 1, only the Pi connection will be deleted. Otherwise, the printer will be permanently deleted.<br>**Default: 0** |

### Response

| Field     | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## List one-click printers

<aside class="notice">
  This endpoint requires the <b>Pro</b> plan.
</aside>

| Required permission |
|---------------------|
| `print_queue`       |

```shell
curl https://api.simplyprint.io/{id}/printers/OneClickPrint?pIds=1234,1235 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "bedsMustBeCleared": true,
  "autoAddAvailable": true,
  "settings": {
    ...
  },
  "canEditSettings": true,
  "hasQueue": true,
  "custom_fields": [
    ...
  ],
  "queue": [
    ...
  ]
}
```

`GET /{id}/printers/OneClickPrint?pid=1234,1235`

#### Query parameters

| Parameter | Type      | Required | Description                              |
|-----------|-----------|----------|------------------------------------------|
| `pid`     | integer[] | yes      | Printers you want to retrieve data about |

### Response

| Parameter           | Type    | Description                                     |
|---------------------|---------|-------------------------------------------------|
| `status`            | boolean | `true` if the request was successful.           |
| `message`           | string  | Error message if `status` is `false`.           |
| `bedsMustBeCleared` | boolean | `true` if beds must be cleared before printing. |
| `autoAddAvailable`  | boolean | `true` if auto-discover printers is available.  |
| `settings`          | object  | Queue auto-start settings.                      |
| `canEditSettings`   | boolean | `true` if the user can edit settings.           |
| `hasQueue`          | boolean | `true` if the company has a print queue.        |
| `custom_fields`     | array   | Array of custom fields for the print queue.     |
| `queue`             | array   | Array of next items in the print queue.         |

## AutoPrint enable / disable

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permission |
|---------------------|
| `autoprint_manage`  |

```shell
curl https://api.simplyprint.io/{id}/printers/autoprint/SetEnabled \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "on": true
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/printers/autoprint/SetEnabled`

#### Request body

| Parameter | Type    | Required | Description                                                  |
|-----------|---------|----------|--------------------------------------------------------------|
| `on`      | boolean | yes      | Set to `true` to enable autoprint, or `false` to disable it. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## AutoPrint check state

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permission |
|---------------------|
| `autoprint_manage`  |

```shell
curl https://api.simplyprint.io/{id}/printers/autoprint/CheckState \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "printers": [
    {
      ...
    }
  ]
}
```

`GET /{id}/printers/autoprint/CheckState`

### Response

| Parameter                                    | Type        | Description                                                            |
|----------------------------------------------|-------------|------------------------------------------------------------------------|
| `status`                                     | boolean     | `true` if the request was successful.                                  |
| `message`                                    | string      | Error message if `status` is `false`.                                  |
| `printers`                                   | array       | Array of printers along with their AutoPrint status.                   |
| `printers[].printer`                         | integer     | The printer id.                                                        |
| `printers[].ready`                           | boolean     | Whether the printer is ready.                                          |
| `printers[].issues`                          | array       | An array of issues with the printer.                                   |
| `printers[].state`                           | object      | The state of the printer.                                              |
| `printers[].state.awaitingBedCool`           | boolean     | True if the printer is awaiting the bed to cool down.                  |
| `printers[].state.awaitingSecondsPass`       | boolean     | True if the printer is awaiting a specified number of seconds to pass. |
| `printers[].state.awaitingManualClear`       | boolean     | True if the printer is awaiting manual clearance.                      |
| `printers[].state.maxCyclesReached`          | boolean     | True if the printer has reached the maximum number of print cycles.    |
| `printers[].state.waitingForSystem`          | boolean     | True if the printer is waiting for the system.                         |
| `printers[].state.awaitingMatchingQueueItem` | boolean     | True if the printer is awaiting a matching queue item.                 |
| `printers[].nextItem`                        | object/null | The next queue item formatted for the printer.                         |

## AutoPrint get settings

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permission |
|---------------------|
| `autoprint_manage`  |

```shell
curl https://api.simplyprint.io/{id}/printers/autoprint/GetAutoPrintSettings \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "gcode": "...",
  "printer_settings": {
    ...
  },
  "printer_has_settings": true,
  "account_settings": {
    ...
  },
  "account_has_settings": true,
  "queue_match_settings": {
    ...
  },
  "can_macro": true
}
```

`GET /{id}/printers/autoprint/GetAutoPrintSettings`

### Response

| Parameter              | Type    | Description                                                                  |
|------------------------|---------|------------------------------------------------------------------------------|
| `status`               | boolean | `true` if the request was successful.                                        |
| `message`              | string  | Error message if `status` is `false`.                                        |
| `gcode`                | string  | G-code for clearing the auto print settings.                                 |
| `printer_settings`     | object  | The auto print settings for the printer.                                     |
| `printer_has_settings` | boolean | `true` if the printer has auto print settings.                               |
| `account_settings`     | object  | The auto print settings for the account.                                     |
| `account_has_settings` | boolean | `true` if the account has auto print settings.                               |
| `queue_match_settings` | object  | The queue match criteria settings for the account.                           |
| `can_macro`            | boolean | `true` if the user has permission to manage G-code profiles for the company. |

## AutoPrint save settings

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permission |
|---------------------|
| `autoprint_manage`  |

```shell
curl https://api.simplyprint.io/{id}/printers/autoprint/SaveAutoPrintSettings \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -H 'Content-Type: application/json' \
  -d '{
    "useDefault": true,
    "saveAsDefault": false,
    "bedReleaseTemp": 60,
    "autoReleaseTime": 3600,
    "maxPrints": 100,
    "ackNoGcode": true,
    "method": "loop"
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/printers/autoprint/SaveAutoPrintSettings`

### Request Body

| Parameter         | Type    | Description                                                                               |
|-------------------|---------|-------------------------------------------------------------------------------------------|
| `useDefault`      | boolean | `true` to use company defaults, `false` to use custom settings.                           |
| `saveAsDefault`   | boolean | `true` to save the settings as company defaults.                                          |
| `bedReleaseTemp`  | integer | Temperature at which the bed releases the print.                                          |
| `autoReleaseTime` | integer | Time in seconds after which the print is automatically released.                          |
| `maxPrints`       | integer | Maximum number of prints before requiring manual intervention.                            |
| `ackNoGcode`      | boolean | `true` to acknowledge no G-code is required.                                              |
| `method`          | string  | Method to use for auto print settings. One of `loop`, `jobox`, `3dque`, `belt`, `pushoff` |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## AutoPrint get gcode templates

| Required permission |
|---------------------|
| `autoprint_manage`  |

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

```shell
curl https://api.simplyprint.io/{id}/printers/autoprint/GetGcodeTemplates \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "methods": [
    {
      "name": "loop",
      "gcode": "..."
    },
    {
      "name": "jobox",
      "gcode": "..."
    },
    {
      "name": "3dque",
      "gcode": "..."
    },
    {
      "name": "belt",
      "gcode": "..."
    },
    {
      "name": "pushoff",
      "gcode": "..."
    }
  ]
}
```

`GET /{id}/printers/autoprint/GetGcodeTemplates`

### Response

| Parameter         | Type    | Description                                      |
|-------------------|---------|--------------------------------------------------|
| `status`          | boolean | `true` if the request was successful.            |
| `message`         | string  | Error message if `status` is `false`.            |
| `methods`         | array   | Array of G-code templates for different methods. |
| `methods[].name`  | string  | Name of the method.                              |
| `methods[].gcode` | string  | G-code template for the method.                  |

## AutoPrint set cleared beds amount

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permission |
|---------------------|
| `autoprint_manage`  |

```shell
curl https://api.simplyprint.io/{id}/printers/autoprint/SetClearedBedsAmount \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -H 'Content-Type: application/json' \
  -d '{
    "amount": 10
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

`POST /{id}/printers/autoprint/SetClearedBedsAmount`

### Request Body

| Parameter | Type      | Description                                                                         |
|-----------|-----------|-------------------------------------------------------------------------------------|
| `pid`     | integer[] | The ID(s) of the printer(s) to set the cleared beds amount for.                     |
| `amount`  | integer   | The number of cleared beds to set for the printer(s). Must be between 0 and 100000. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Cancel reasons

| ID | Description                        |
|----|------------------------------------|
| 1  | Print failed                       |
| 2  | Regretted print                    |
| 3  | No filament extruded / nozzle clog |
| 4  | Fell of the plate                  |
| 5  | Not enough filament                |
| 6  | Other                              |
