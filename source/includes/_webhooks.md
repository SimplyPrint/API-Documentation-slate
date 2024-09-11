# Webhooks

<aside class="notice">
  This feature is only available on the <b>Print Farm</b> plan.
</aside>

Webhooks allow you to build or set up integrations that subscribe to certain events on SimplyPrint.
When one of those events is triggered, we'll send an HTTP POST payload to the webhook's configured URL.

## List webhooks

`GET /{id}/webhooks/Get`

> Example request

```shell
curl "https://api.simplyprint.io/{id}/webhooks/Get" \
    -H 'accept: application/json' \
    -H "X-API-KEY: {API_KEY}"
```

> Example response

```json
{
  "status": true,
  "message": null,
  "data": [
    {
      "id": 11,
      "created_by": {
        "id": 12640,
        "first_name": "John",
        "last_name": "Doe",
        "avatar": "https://example.com/avatar.jpg"
      },
      "name": "Example Webhook",
      "url": "https:\/\/actions.nasa.gov\/print_events",
      "events": [
        "job.started",
        "job.paused",
        "job.resumed",
        "job.cancelled",
        "job.done",
        "job.failed",
        "job.bed_cleared"
      ],
      "secret": null,
      "enabled": true,
      "oauth_client_id": null,
      "created_at": "2024-09-07T18:16:51+00:00",
      "updated_at": "2024-09-07T18:16:51+00:00"
    }
  ]
}
```

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------| 
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |
| `data`    | array   | Array of webhooks.                  |

## Create or update a webhook

`POST /{id}/webhooks/Create`

> Example request

```shell
curl -X POST \
https://api.simplyprint.io/{id}/webhooks/Create \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-H 'X-API-KEY: {API_KEY}' \
-d '{ "name": "Order Created", "description": "Triggers when an order is created", "url": "https://example.com/webhook", "secret": "mysecret", "enabled": true, "events": ["order.created", "order.updated"] }'
```

> Example response

```json
{
  "status": true,
  "message": null,
  "webhook": {
    "id": 123,
    "name": "Order Created",
    "description": "Triggers when an order is created",
    "url": "https://example.com/webhook",
    "secret": "mysecret",
    "enabled": true,
    "events": [
      "order.created",
      "order.updated"
    ],
    "created_by": "User Name",
    "company": "Company Name"
  }
}
```

### Request

| Parameter     | Type    | Required | Description                                                                                                                             |
|---------------|---------|----------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `id`          | integer | no       | The ID of the webhook. Required if updating an existing webhook.                                                                        |
| `name`        | string  | yes      | The name of the webhook.                                                                                                                |
| `description` | string  | no       | A description of the webhook.                                                                                                           |
| `url`         | string  | yes      | The URL where the webhook will send requests. Must be a valid URL.                                                                      |
| `secret`      | string  | no       | A secret key used to sign the webhook payloads.                                                                                         |
| `enabled`     | boolean | no       | Whether the webhook is enabled or not. Default is `false`.                                                                              |
| `events`      | array   | yes      | A list of events that the webhook will listen to. Must be at least one. Each event should match a valid [WebhookEvent](#webhook-event). |

### Response

| Parameter | Type    | Description                                                                                         |
|-----------|---------|-----------------------------------------------------------------------------------------------------|
| `status`  | boolean | True if the request was successful.                                                                 |
| `message` | string  | Error message if `status` is false.                                                                 |
| `webhook` | object  | Details of the created or updated webhook. Contains fields like `id`, `name`, `url`, `events`, etc. |

## Delete a webhook

`POST /{id}/webhooks/Delete`

> Example request

```shell
curl -X POST \
https://api.simplyprint.io/{id}/webhooks/Delete \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-H 'X-API-KEY: {API_KEY}' \
-d '{"id": 123}'
```

> Example response

```json
{
  "status": true,
  "message": null
}
```

### Request

| Parameter | Type    | Required | Description                      |
|-----------|---------|----------|----------------------------------|
| `id`      | integer | yes      | The ID of the webhook to delete. |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Enable or disable a webhook

`POST /{id}/webhooks/SetEnabled`

> Example request

```shell
curl -X POST \
https://api.simplyprint.io/{id}/webhooks/SetEnabled \
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

| Parameter | Type    | Required | Description                                    |
|-----------|---------|----------|------------------------------------------------|
| `id`      | integer | yes      | The ID of the webhook to enable or disable.    |
| `enabled` | boolean | yes      | Set to `true` to enable or `false` to disable. |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## Manually trigger webhook

The event type will be `test`.

`POST /{id}/webhooks/TriggerTestWebhook`

> Example request

```shell
curl -X POST \
https://api.simplyprint.io/{id}/webhooks/TriggerTestWebhook \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-H 'X-API-KEY: {API_KEY}' \
-d '{"id": 123}'
```

> Example response

```json
{
  "status": true,
  "message": null
}
```

### Request

| Parameter | Type    | Required | Description                       |
|-----------|---------|----------|-----------------------------------|
| `id`      | integer | yes      | The ID of the webhook to trigger. |

### Response

| Parameter | Type    | Description                         |
|-----------|---------|-------------------------------------|
| `status`  | boolean | True if the request was successful. |
| `message` | string  | Error message if `status` is false. |

## WebhookEvent

| Event Name                        | Description                                                 |
|-----------------------------------|-------------------------------------------------------------|
| `job.started`                     | Triggered when a job is started.                            |
| `job.paused`                      | Triggered when a job is paused.                             |
| `job.resumed`                     | Triggered when a job is resumed.                            |
| `job.cancelled`                   | Triggered when a job is cancelled.                          |
| `job.done`                        | Triggered when a job is done.                               |
| `job.failed`                      | Triggered when a job has failed.                            |
| `job.bed_cleared`                 | Triggered when the bed is cleared.                          |
| `printer.autoprint_state_changed` | Triggered when the autoprint state of a printer is changed. |
| `printer.nozzle_size_changed`     | Triggered when the nozzle size of a printer is changed.     |
| `printer.material_changed`        | Triggered when the material of a printer is changed.        |
| `printer.custom_tag_assigned`     | Triggered when a custom tag is assigned to a printer.       |
| `printer.custom_tag_detached`     | Triggered when a custom tag is detached from a printer.     |
| `company.autoprint_state_changed` | Triggered when the autoprint state of a company is changed. |

