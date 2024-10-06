# GCode Macros

## Get overview

```shell
curl https://api.simplyprint.io/{id}/gcode_macros/GetOverview \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "gcodeCompletions": [
    {
      "label": "G0",
      "type": "variable",
      "info": "Add a straight line movement to the planner",
      "detail": "X, Y, Z, E, F, S"
    }
  ],
  "macro_groups": [
    {
      "name": "<i class='fas fa-infinity'><\/i> AutoPrint <i>(advanced)<\/i>",
      "desc": "The AutoPrint feature can automatically start prints. Here you define what Gcode should be executed to make sure the bed is clear.",
      "macros": [
        "is_autoprint_clear"
      ],
      "nopersonal": true
    }
  ],
  "macros_cards": {
    "is_start": {
      "name": "Start print",
      "description": "GCODE to be executed at start of print when using our slicer"
    }
  },
  "macros": {
    "company": [
      {
        "gcodes": {
          "printer_225": [
            "{snippet:2731}"
          ],
          "printer_10063": null,
          "printer_10372": [
            "{snippet:2796}"
          ]
        },
        "type": "is_start"
      }
    ],
    "personal": null,
    "sp": [
      {
        "gcodes": {
          "type_0": [
            "{snippet:26}"
          ],
          "type_236": [
            "{snippet:1103}",
            "{snippet:26}"
          ]
        },
        "type": "is_pause"
      }
    ]
  },
  "snippets": {
    "company": [
      {
        "id": 28,
        "name": "Indentify Ender 5",
        "description": "Identify printer specifically for Ender 5",
        "priority": 0,
        "created": "2020-04-22T09:19:37+00:00"
      }
    ]
  },
  "variables": {
    "bed_x": "Printer bed X length in mm",
    "bed_y": "Printer bed Y length in mm"
  },
  "slicerVariables": {
    "temp": "Hot end temperature",
    "bed_temp": "Bed temperature",
    "fan_speed": "Active cooling fan speed (usually 0-255)"
  }
}
```

`GET /{id}/gcode_macros/GetOverview`

### Response

See example response.

## Get snippet

```shell
curl https://api.simplyprint.io/{id}/gcode_macros/GetSnippet?id=123 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Success Response

```json
{
  "status": true,
  "message": null,
  "snippet": {
    "id": 123,
    "name": "Example Snippet",
    "description": "This is an example snippet",
    "priority": 0,
    "gcode": "G1 X10 Y10"
  }
}
```

`GET /gcode_macros/GetSnippet`

### Request Parameters

| Parameter | Type    | Description                        |
|-----------|---------|------------------------------------|
| `id`      | integer | The ID of the snippet to retrieve. |

### Response

| Parameter             | Type    | Description                            |
|-----------------------|---------|----------------------------------------|
| `status`              | boolean | `true` if the request was successful.  |
| `message`             | string  | Error message if `status` is `false`.  |
| `snippet`             | object  | The details of the snippet.            |
| `snippet.id`          | integer | Unique identifier for the snippet.     |
| `snippet.name`        | string  | Name of the snippet.                   |
| `snippet.description` | string  | Description of the snippet.            |
| `snippet.priority`    | integer | Priority of the snippet.               |
| `snippet.gcode`       | string? | GCode content of the snippet (if any). |

## Save snippet

```shell
curl -X POST https://api.simplyprint.io/{id}/gcode_macros/SaveSnippet?id=123&type=private \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "name": "Updated Snippet",
    "description": "This is an updated snippet.",
    "gcode": ["G1 X10 Y10", "G1 X20 Y20"]
  }'
```

> Success Response

```json
{
  "status": true,
  "message": null,
  "new_data": {
    // Updated data of the GCODE snippets
  }
}
```

`POST /gcode_macros/SaveSnippet`

### Request Parameters

| Parameter | Type    | Description                                                            |
|-----------|---------|------------------------------------------------------------------------|
| `id`      | integer | The ID of the snippet to update (optional for creating a new snippet). |
| `type`    | string  | The type of the snippet (`org`, `private`, `public`).                  |

### Request Body

| Parameter     | Type   | Description                        |
|---------------|--------|------------------------------------|
| `name`        | string | The name of the GCODE snippet.     |
| `description` | string | The description of the snippet.    |
| `gcode`       | array  | The GCODE commands of the snippet. |
| `gcode.*`     | string | A single GCODE command.            |

### Response

| Parameter  | Type    | Description                                          |
|------------|---------|------------------------------------------------------|
| `status`   | boolean | `true` if the request was successful.                |
| `message`  | string  | Error message if `status` is `false`.                |
| `created`  | integer | The ID of the newly created snippet (if applicable). |
| `new_data` | object  | The updated data of the GCODE snippets.              |

## Delete snippet

```shell
curl -X POST https://api.simplyprint.io/{id}/gcode_macros/DeleteSnippet?id=123 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success Response

```json
{
  "status": true,
  "message": null,
  "new_data": {
    // Updated data of the GCODE snippets
  }
}
```

`POST /gcode_macros/DeleteSnippet`

### Request Parameters

| Parameter | Type    | Description                                                                           |
|-----------|---------|---------------------------------------------------------------------------------------|
| `id`      | integer | The ID of the snippet to delete (required if `ids` is not provided).                  |
| `ids`     | string  | Comma-separated list of IDs of snippets to delete (required if `id` is not provided). |

### Response

| Parameter  | Type    | Description                             |
|------------|---------|-----------------------------------------|
| `status`   | boolean | `true` if the request was successful.   |
| `message`  | string  | Error message if `status` is `false`.   |
| `new_data` | object  | The updated data of the GCODE snippets. |

## Get macro

```shell
curl https://api.simplyprint.io/{id}/gcode_macros/GetMacroGcode?macro=is_start&printer=123&model=456&org=true&private=false&public=true \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
```

> Success Response

```json
{
  "status": true,
  "message": null,
  "gcode": [
    "G1 X10 Y10",
    "G1 X20 Y20"
  ],
  "multiple": false
}
```

`GET /gcode_macros/GetMacroGcode`

### Request Parameters

| Parameter | Type    | Description                                                                  |
|-----------|---------|------------------------------------------------------------------------------|
| `macro`   | string  | The type of the macro. Must be one of the enums defined in `GcodeMacroType`. |
| `printer` | integer | The ID of the printer.                                                       |
| `model`   | integer | The ID of the printer model.                                                 |
| `org`     | boolean | Whether to include organization macros.                                      |
| `private` | boolean | Whether to include private macros.                                           |
| `public`  | boolean | Whether to include public macros.                                            |

### Response

| Parameter  | Type    | Description                           |
|------------|---------|---------------------------------------|
| `status`   | boolean | `true` if the request was successful. |
| `message`  | string  | Error message if `status` is `false`. |
| `gcode`    | array   | The GCODE commands of the macro.      |
| `multiple` | boolean | Whether multiple GCODEs are returned. |

## Save macro

```shell
curl -X POST https://api.simplyprint.io/gcode_macros/SaveMacro?type=private \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
  "type": "is_start",
  "gcodes": {"printer_225": ["G1 X10 Y10"], "type_123": ["G1 X20 Y20", "{snippet:123}"]}
  }'
```

> Success Response

```json
{
  "status": true,
  "message": null
}
```

| Required permission | Description                         |
|---------------------|-------------------------------------|
| `gcode_profiles`    | Required if macro is of type `org`. |

`POST /gcode_macros/SaveMacro`

### Request Parameters

| Parameter | Type   | Description             |
|-----------|--------|-------------------------|
| `type`    | string | One of `org`, `private` |

### Request Body

| Parameter             | Type   | Description                                                                             |
|-----------------------|--------|-----------------------------------------------------------------------------------------|
| `type`                | string | One of `is_start`, `is_end`, `is_cancel`, `is_pause`, `is_resume`, `is_autorpint_clear` |
| `gcodes`              | object | Content of the macro.                                                                   |
| `gcodes.printer_{id}` | array  | GCODE commands for the printer with ID `{id}`.                                          |
| `gcodes.type_{id}`    | array  | GCODE commands for the printer model with ID `{id}`.                                    |
| `gcodes.*[]`          | string | Can be a GCODE command or `{snippet:{id}}` to reference a snippet.                      |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Arrange gcode order in macro

```shell
curl -X POST https://api.simplyprint.io/gcode_macros/Arrange \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "order": [4,2,3,1]
  }'
```

> Success Response

```json
{
  "status": true,
  "message": null
}
```

`POST /gcode_macros/Arrange`

### Request Parameters

| Parameter | Type    | Description                                   |
|-----------|---------|-----------------------------------------------|
| `order`   | array   | An array of snippet IDs in the desired order. |
| `order.*` | integer | A single snippet ID.                          |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
