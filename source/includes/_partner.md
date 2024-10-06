# Partner

<aside class="notice">
This API is only available to partners. 
</aside>

## Get companies

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/GetCompanies \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "page": 1,
    "page_size": 10,
    "search": "example",
    "sort_id": "name",
    "sort_dir": "asc"
  }'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "data": [
    // List of companies
  ],
  "total": 100,
  "page_amount": 10
}
```

<aside class="notice">
This endpoint is only available to partners. 
</aside>

`POST /{id}/partner/GetCompanies`

### Request Body

| Parameter   | Type    | Description                                       |
|-------------|---------|---------------------------------------------------|
| `page`      | integer | The page number to retrieve.                      |
| `page_size` | integer | The number of items per page (between 1 and 100). |
| `search`    | string  | Search term to filter companies (max 50 chars).   |
| `sort_id`   | string  | The field to sort by.                             |
| `sort_dir`  | string  | The direction to sort (asc or desc).              |

### Response

| Parameter     | Type    | Description                           |
|---------------|---------|---------------------------------------|
| `status`      | boolean | `true` if the request was successful. |
| `message`     | string  | Error message if `status` is `false`. |
| `data`        | array   | The list of companies.                |
| `total`       | integer | The total number of companies.        |
| `page_amount` | integer | The total number of pages.            |

## Get company

```shell
curl https://api.simplyprint.io/{id}/partner/GetCompany?company=123&initial=true \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "data": {
    "id": 123,
    "name": "Example Company",
    "vat": "12345678",
    "ean": "1234567890123",
    "trial_until": "2023-12-31T23:59:59",
    "country": "US",
    "uid": "unique-id",
    "created": "2023-01-01T00:00:00",
    "active": true,
    "sponsored": false,
    "public_edu": true,
    "plan": "premium",
    "type": "business",
    "max_printers": 10,
    "max_users": 50,
    "user_max_gb": 100,
    "address": "123 Example St",
    "contact_person": "John Doe",
    "contact_email": "john.doe@example.com",
    "contact_phone": "+1234567890",
    "user_count": 25,
    "printers_count": 5,
    "online_printers": 3,
    "week_jobs": 10,
    "total_jobs": 100,
    "filament": 20,
    "success_jobs": 8,
    "online_user_count": 15,
    "user_space_used": 50
  },
  "users_data": {
    // Initial user data if applicable
  }
}
```

<aside class="notice">
This endpoint is only available to partners. 
</aside>

`GET /{id}/partner/GetCompany`

### Request Parameters

| Parameter | Type    | Description                                      |
|-----------|---------|--------------------------------------------------|
| `company` | integer | The ID of the company to retrieve.               |
| `initial` | boolean | Whether to include initial user data (optional). |

### Response

| Parameter    | Type    | Description                               |
|--------------|---------|-------------------------------------------|
| `status`     | boolean | `true` if the request was successful.     |
| `message`    | string  | Error message if `status` is `false`.     |
| `data`       | object  | The company details.                      |
| `users_data` | object  | Initial user data if `initial` is `true`. |

## Get overview stats

```shell
curl https://api.simplyprint.io/{id}/partner/GetOverviewStats \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "total_active_managed_companies": 10,
  "total_trial_managed_companies": 2,
  "total_inactive_managed_companies": 3,
  "total_printers": 50,
  "total_online_printers": 30,
  "total_print_jobs": 200,
  "total_print_jobs_last_7_days": 20,
  "total_users": 100,
  "total_users_online_last_7_days": 15
}
```

<aside class="notice">
This endpoint is only available to partners. 
</aside>

`GET /{id}/partner/GetOverviewStats`

### Response

| Parameter                          | Type    | Description                                                |
|------------------------------------|---------|------------------------------------------------------------|
| `status`                           | boolean | `true` if the request was successful.                      |
| `message`                          | string  | Error message if `status` is `false`.                      |
| `total_active_managed_companies`   | integer | Total number of active managed companies.                  |
| `total_trial_managed_companies`    | integer | Total number of managed companies in trial.                |
| `total_inactive_managed_companies` | integer | Total number of inactive managed companies.                |
| `total_printers`                   | integer | Total number of printers for all managed companies.        |
| `total_online_printers`            | integer | Total number of online printers for all managed companies. |
| `total_print_jobs`                 | integer | Total number of print jobs for all managed companies.      |
| `total_print_jobs_last_7_days`     | integer | Total number of print jobs in the last 7 days.             |
| `total_users`                      | integer | Total number of users for all managed companies.           |
| `total_users_online_last_7_days`   | integer | Total number of users online in the last 7 days.           |

## Create/update company

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/CreateCompany \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "name": "Example Company",
    "uid": "unique-id",
    "active": 1,
    "trial_until": "2023-12-31",
    "public_edu": true,
    "extra_printers": 5,
    "extra_users": 50,
    "extra_gb": 100,
    "active_plan": 5,
    "type": 1,
    "contact_email": "contact@example.com",
    "contact_phone": "+1234567890",
    "att_person": "John Doe",
    "uni_login": true,
    "uni_login_ids": "id1,id2",
    "address_stuff": {
      "street": "123 Example St",
      "city": "Example City",
      "country": "US",
      "postal_code": "12345"
    }
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

<aside class="notice">
This endpoint is only available to partners. 
</aside>

`POST /{id}/partner/CreateCompany`

### Request Parameters

| Parameter | Type    | Description                               |
|-----------|---------|-------------------------------------------|
| `edit`    | integer | The ID of the company to edit (optional). |

### Request Body

| Parameter        | Type    | Description                                                          |
|------------------|---------|----------------------------------------------------------------------|
| `name`           | string  | The name of the company (required, max 128 characters).              |
| `uid`            | string  | The unique identifier for the company (required, max 32 characters). |
| `active`         | integer | Whether the company is active (required, 0 or 1).                    |
| `trial_until`    | date    | The trial end date in `Y-m-d` format (optional).                     |
| `public_edu`     | boolean | Whether the company is a public educational institution (required).  |
| `extra_printers` | integer | The number of extra printers (required, min 0).                      |
| `extra_users`    | integer | The number of extra users (required, min 0).                         |
| `extra_gb`       | integer | The amount of extra storage in GB (required, min 0).                 |
| `active_plan`    | integer | The active plan ID (required, must be 4 or 5).                       |
| `type`           | integer | The type of company (required, must be 0, 1, 2, or 3).               |
| `contact_email`  | string  | The contact email for the company (required, valid email format).    |
| `contact_phone`  | string  | The contact phone number (optional, max 32 characters).              |
| `att_person`     | string  | The name of the contact person (required, max 128 characters).       |
| `uni_login`      | boolean | Whether UniLogin is enabled (optional).                              |
| `uni_login_ids`  | string  | Comma-separated UniLogin IDs (optional).                             |
| `address_stuff`  | array   | The address details (required).                                      |

### Response

| Parameter | Type    | Description                               |
|-----------|---------|-------------------------------------------|
| `status`  | boolean | `true` if the request was successful.     |
| `message` | string  | Error message if `status` is `false`.     |
| `id`      | integer | The ID of the created or updated company. |

## Delete company

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/DeleteCompany?company=123 \
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
This endpoint is only available to partners. 
</aside>

`POST /{id}/partner/DeleteCompany`

### Request Parameters

| Parameter | Type    | Description                      |
|-----------|---------|----------------------------------|
| `company` | integer | The ID of the company to delete. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Validate printer (partner-wide printer search)

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/move/ValidatePrinter \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "ids": [1, 2, 3],
    "search": "example",
    "not_in": [4, 5]
  }'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "items": [
    {
      "id": 1,
      "name": "Printer 1",
      "sku": "SKU123",
      "org_id": 10,
      "image": "https://example.com/printer1.jpg"
    },
    {
      "id": 2,
      "name": "Printer 2",
      "sku": "SKU456",
      "org_id": 11,
      "image": "https://example.com/printer2.jpg"
    }
  ]
}
```

<aside class="notice">
This endpoint is only available to partners.
</aside>

Searches for printers across all partner-companies.
Useful for finding printers to move between companies.

`POST /{id}/partner/move/ValidatePrinter`

### Request Body

| Parameter  | Type    | Description                                               |
|------------|---------|-----------------------------------------------------------|
| `ids`      | array   | List of printer IDs to search (optional).                 |
| `ids.*`    | integer | Each printer ID must be an integer.                       |
| `search`   | string  | Search term to filter printers (optional, max 256 chars). |
| `not_in`   | array   | List of printer IDs to exclude (optional).                |
| `not_in.*` | integer | Each printer ID to exclude must be an integer.            |

### Response

| Parameter        | Type    | Description                               |
|------------------|---------|-------------------------------------------|
| `status`         | boolean | `true` if the request was successful.     |
| `message`        | string  | Error message if `status` is `false`.     |
| `items`          | array   | List of validated printers.               |
| `items.*.id`     | integer | The ID of the printer.                    |
| `items.*.name`   | string  | The name of the printer.                  |
| `items.*.sku`    | string  | The SKU of the printer.                   |
| `items.*.org_id` | integer | The ID of the company owning the printer. |
| `items.*.image`  | string  | URL of the printer's image.               |

## Move printer

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/move/MovePrinter \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "moveTo": 123,
    "moveToGroup": 456,
    "ids": [1, 2, 3]
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

<aside class="notice">
This endpoint is only available to partners. 
</aside>

Find printers to move with `ValidatePrinter` endpoint.

`POST /{id}/partner/move/MovePrinter`

### Request Parameters

| Parameter     | Type    | Description                                    |
|---------------|---------|------------------------------------------------|
| `moveTo`      | integer | The ID of the target company (required).       |
| `moveToGroup` | integer | The ID of the target printer group (required). |
| `ids`         | array   | List of printer IDs to move (required).        |
| `ids.*`       | integer | Each printer ID must be an integer.            |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Validate filament (partner-wide filament search)

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/move/ValidateFilament \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "ids": [1, 2, 3],
    "search": "example filament",
    "not_in": [4, 5]
  }'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "items": [
    {
      "id": 1,
      "uniq_id": "UNIQ123",
      "color_name": "Red",
      "color_hex": "#FF0000",
      "org_id": 10,
      "brand": "Example Brand",
      "type_id": 1,
      "type_name": "PLA"
    }
  ]
}
```

<aside class="notice">
This endpoint is only available to partners.
</aside>

Searches for filaments across all partner-companies.
Useful for finding filaments to move between companies.

`POST /{id}/partner/move/ValidateFilament`

### Request Body

| Parameter  | Type    | Description                                               |
|------------|---------|-----------------------------------------------------------|
| `ids`      | array   | List of filament IDs to search (optional).                |
| `ids.*`    | integer | Each filament ID must be an integer.                      |
| `search`   | string  | Search term to filter filament (optional, max 256 chars). |
| `not_in`   | array   | List of filament IDs to exclude (optional).               |
| `not_in.*` | integer | Each filament ID to exclude must be an integer.           |

### Response

| Parameter            | Type    | Description                                |
|----------------------|---------|--------------------------------------------|
| `status`             | boolean | `true` if the request was successful.      |
| `message`            | string  | Error message if `status` is `false`.      |
| `items`              | array   | List of validated filaments.               |
| `items.*.id`         | integer | The ID of the filament.                    |
| `items.*.uniq_id`    | string  | The unique ID of the filament.             |
| `items.*.color_name` | string  | The name of the filament color.            |
| `items.*.color_hex`  | string  | The hex code of the filament color.        |
| `items.*.org_id`     | integer | The ID of the company owning the filament. |
| `items.*.brand`      | string  | The brand of the filament.                 |
| `items.*.type_id`    | integer | The ID of the filament type.               |
| `items.*.type_name`  | string  | The name of the filament type.             |

## Move filament

```shell
curl -X POST https://api.simplyprint.io/{id}/partner/move/MoveFilament \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "moveTo": 123,
    "moveToFilamentType": 456,
    "filamentMoveTypeClean": false,
    "ids": [1, 2, 3]
  }'
```

> Success response

```json
{
  "status": true,
  "message": null
}
```

<aside class="notice">
This endpoint is only available to partners. 
</aside>

Find filament to move with `ValidateFilament` endpoint.

`POST /{id}/partner/move/MoveFilament`

### Request Parameters

| Parameter               | Type    | Description                                                                      |
|-------------------------|---------|----------------------------------------------------------------------------------|
| `moveTo`                | integer | The ID of the target company (required).                                         |
| `moveToFilamentType`    | integer | The ID of the target filament type (required).                                   |
| `filamentMoveTypeClean` | boolean | Whether to only change the filament company, or also change the type (optional). |
| `ids`                   | array   | List of printer IDs to move (required).                                          |
| `ids.*`                 | integer | Each printer ID must be an integer.                                              |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
