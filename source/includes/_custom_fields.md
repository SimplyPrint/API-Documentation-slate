# Custom Fields

<aside class="notice">
  This feature requires the <b>Print Farm</b> plan.
</aside>

Custom Fields allow you to add your own data-fields inside SimplyPrint.

[Read more about Custom Fields on our Helpdesk](https://help.simplyprint.io/en/article/all-about-the-custom-fields-feature-4dd5if/)

## List custom fields

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permissions   |
|------------------------|
| `custom_fields_manage` |

`POST /{id}/custom_fields/Get`

> Example request

```shell
curl -X POST https://api.simplyprint.io/{id}/custom_fields/Get \
    -H 'accept: application/json' \
    -H "X-API-KEY: {API_KEY}" \
    -F "page=1" -F "page_size=10"
```

> Example response

```json
{
  "status": true,
  "message": null,
  "data": [
    {
      "id": 7,
      "fieldId": "student_id",
      "fieldLabel": "Student ID",
      "fieldDescription": "",
      "fieldPlaceholder": null,
      "fieldType": "text",
      "fieldOptions": null,
      "category": "print",
      "subCategory": [
        "user_file",
        "print_queue",
        "print_job"
      ],
      "required": false,
      "enabled": true,
      "defaultValue": null,
      "visibleRequiredPermissions": null,
      "editRequiredPermissions": null,
      "visibleToGroups": [],
      "editableByGroups": [],
      "validation": null,
      "createdByUser": -1,
      "user": "John Doe",
      "forPrinters": [],
      "forModels": [],
      "forGroups": [],
      "showOnRegistration": null,
      "showBeforeStartPrint": null,
      "position": 0,
      "created": "2024-09-07T13:46:59+00:00",
      "updated": "2024-09-07T13:46:59+00:00"
    }
  ],
  "page_amount": 1,
  "total": 1
}
```

### Request

| Parameter   | Type    | Required | Description                                                                                                     |
|-------------|---------|----------|-----------------------------------------------------------------------------------------------------------------|
| `page`      | integer | yes      | Which page to show                                                                                              |
| `page_size` | integer | yes      | Amount of items per page (1-100)                                                                                |
| `search`    | string  | no       | The search filter to apply                                                                                      |
| `sort_id`   | string  | no       | What key to sort on (id, fieldId, fieldLabel, fieldDescription, fieldType, category, enabled, created, updated) |
| `sort_dir`  | string  | no       | Sort direction (`asc`, `desc`)                                                                                  |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------| 
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |
| `data`    | array   | List of custom fields.              |

## Create or update a custom field

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permissions   |
|------------------------|
| `custom_fields_manage` |

`POST /{id}/custom_fields/Save`

> Example request

```shell
curl -X POST https://api.simplyprint.io/{id}/custom_fields/Save \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{"category":"print","subCategory":["user_file","print_queue","print_job"],"fieldType":"text","fieldId":"student_id","fieldLabel":"Student ID","required":false,"enabled":true}'
```

> Example response

```json
{
  "status": true,
  "message": null
}
```

### Request

| Parameter              | Type    | Required | Description                                                                                                                                                                                   |
|------------------------|---------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`                   | integer | no       | If you want to update an existing custom field, specify this                                                                                                                                  |
| `category`             | string  | yes      | One of `print`, `user`, `printer`, `filament`                                                                                                                                                 |
| `subCategory`          | array   | no       | Array of subcategories. Valid subcategories: `print_queue`, `print_job`, `user_file`                                                                                                          |
| `fieldId`              | string  | yes      | ID of the field                                                                                                                                                                               |
| `fieldType`            | string  | yes      | One of `boolean`, `text`, `longtext`, `number`, `date`, `datetime`, `phone`, `email`, `url`, `select`, `multiselect`, `radio`, `checkbox`                                                     |
| `fieldLabel`           | string  | yes      | Label of the field                                                                                                                                                                            |
| `fieldOptions`         | object  | no       | Field options. An object with one entry, `options`, whose value is an array of `{label: string, value: string}`                                                                               |
| `FieldDescription`     | string  | no       | Description of the field                                                                                                                                                                      |
| `fieldPlaceholder`     | string  | no       | Placeholder text for the field                                                                                                                                                                |
| `required`             | boolean | yes      | Whether the field is required                                                                                                                                                                 |
| `defaultValue`         | object  | no       | Default value of the field. Object key should be either `string`, `number`, `boolean`, `date`, `options`, with the appropriate value type                                                     |
| `validation`           | object  | no       | Validation rules for the field. Object keys can be any of `stringRegex`, `stringMinLength`, `stringMaxLength`, `numberAllowDecimals`, `numberMinValue`, `numberMaxValue`, `validationMessage` |
| `forPrinters`          | array   | no       | Array of printer IDs the field should be visible for                                                                                                                                          |
| `forModels`            | array   | no       | Array of model IDs the field should be visible for                                                                                                                                            |
| `forGroups`            | array   | no       | Array of group IDs the field should be visible for                                                                                                                                            |
| `showOnRegistration`   | boolean | no       | Whether the field should be shown on registration                                                                                                                                             |
| `showBeforeStartPrint` | boolean | no       | Whether the field should be shown before starting a print                                                                                                                                     |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------| 
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Enable or disable a custom field

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permissions   |
|------------------------|
| `custom_fields_manage` |

`POST /{id}/custom_fields/SetEnabled`

> Example request

```shell
curl -X POST https://api.simplyprint.io/{id}/custom_fields/SetEnabled \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{"id": 123, "enabled": true}'
```

> Example response

```json
{
  "status": true,
  "message": null
}
```

### Request

| Parameter | Type    | Required | Description                                                               |
|-----------|---------|----------|---------------------------------------------------------------------------|
| `id`      | integer | yes      | ID of the custom field to enable or disable                               |
| `enabled` | boolean | yes      | Whether the custom field should be enabled (`true`) or disabled (`false`) |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Delete custom fields

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

| Required permissions   |
|------------------------|
| `custom_fields_manage` |

`GET /{id}/custom_fields/Delete`

> Example request

```shell
curl https://api.simplyprint.io/{id}/custom_fields/Delete?id=123 \
-H 'accept: application/json' \
-H 'X-API-KEY: {API_KEY}'
```

> Example request with multiple IDs

```shell
curl https://api.simplyprint.io/{id}custom_fields/Delete?ids=123,124,125 \
-H 'accept: application/json' \
-H 'X-API-KEY: {API_KEY}'
```

> Example response

```json
{
  "status": true,
  "message": null
}
```

### Request

| Parameter | Type    | Required                       | Description                                                                                        |
|-----------|---------|--------------------------------|----------------------------------------------------------------------------------------------------|
| `id`      | integer | yes (if `ids` is not provided) | The ID of the custom field to delete.                                                              |
| `ids`     | string  | yes (if `id` is not provided)  | A comma-separated list of custom field IDs to delete. Valid if multiple fields need to be removed. |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Custom field submission

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

`POST /{id}/custom_fields/SubmitValues`

> Example request

```shell
curl -X POST https://api.simplyprint.io/{id}/custom_fields/SubmitValues \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{"category": "printer", "subCategory": "print_job", entityIds: [1234], "values": [{customFieldId: "student_id", value: {"string": "1234567890"}}]}'
```

> Example response

```json
{
  "status": true,
  "message": null
}
```

### Request

| Parameter     | Type   | Required | Description                                                                                                      |
|---------------|--------|----------|------------------------------------------------------------------------------------------------------------------|
| `category`    | string | yes      | One of `print`, `user`, `printer`, `filament`                                                                    |
| `subCategory` | string | yes      | One of `print_queue`, `print_job`, `user_file`                                                                   |
| `entityIds`   | array  | yes      | Array of entity IDs to submit values for                                                                         |
| `values`      | array  | yes      | Array of custom field values to submit. Each value looks like `{customFieldId: string, value: CustomFieldValue}` |

## Custom field submission value

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

The value of a custom field when submitted via the API is an object with a key corresponding to the field type.
See the examples to the right for the different field types.

> A text field would have a value like this:

```json
{
  "string": "1234567890"
}
```

> A number field would have a value like this:

```json
{
  "number": 1234567890
}
```

> A date field would have a value like this:

```json
{
  "date": "2024-09-07"
}
```

> A select field would have a value like this:

```json
{
  "string": "Selected option"
}
```

> A multi-select field would have a value like this:

```json
{
  "options": [
    "Option 1",
    "Option 2"
  ]
}
```
