# Print Jobs

## Get print jobs

```shell
curl https://api.simplyprint.io/{id}/jobs/GetPaginatedPrintJobs \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "page": 1,
  "page_size": 10,
  "printer_ids": [
    385
  ],
  "status": [
    "cancelled",
    "finished"
  ],
  "start_date": "2023-02-28"
}
```

> Success response

```json
{
  "status": true,
  "message": null,
  "data": [
    {
      "id": 549145,
      "uid": "7df103aa-b12c-4b33-8305-b55f91c11a4d",
      "status": "cancelled",
      "cancelReasonType": "5",
      "rating": -2,
      "filename": "Benchy.gcode",
      "startDate": "2023-02-28T21:05:50+00:00",
      "endDate": "2023-02-28T21:06:07+00:00",
      "user": 5933,
      "autoprint": false,
      "outsideSystem": false,
      "printer": 385,
      "filament": "{\"e0\": {\"usage\": 60}}",
      "filUsage": 60,
      "filUsageGram": 0,
      "currentPercentage": 48,
      "printTime": 17,
      "cost": {
        "estimate": false,
        "total_cost": 150,
        "lines": [
          {
            "id": 1,
            "label": "Material usage (account default)",
            "cost": 0.02
          },
          {
            "id": 2,
            "label": "Material markup",
            "cost": null
          },
          {
            "id": 3,
            "label": "Machine run time cost",
            "cost": null
          },
          {
            "id": 4,
            "label": "Energy cost",
            "cost": null
          },
          {
            "id": 5,
            "label": "Labor cost",
            "cost": 1000
          }
        ]
      },
      "queueItem": {
        "id": 1234,
        "user": 51,
        "queueNum": 3
      },
      "customFields": [
        {
          "id": "student_id",
          "value": {
            "string": "1234567890"
          }
        }
      ]
    },
    ...
  ],
  "page_amount": 1
}
```

Get paginated data about ongoing or finished print jobs.

### Request

`POST /{id}/jobs/GetPaginatedPrintJobs`

| Parameter             | Type      | Required | Description                                                                              |
|-----------------------|-----------|----------|------------------------------------------------------------------------------------------|
| `page`                | integer   | yes      | The page number to get.                                                                  |
| `page_size`           | integer   | yes      | The number of items per page. (Between 1 and 100)                                        |
| `printer_types[]`     | integer[] | no       | Array of printer type ids to filter on.                                                  |
| `printer_ids[]`       | integer[] | no       | Array of printer ids to filter on.                                                       |
| `user_ids[]`          | integer[] | no       | Array of user ids to filter on.                                                          |
| `accepted_statuses[]` | string[]  | no       | Array of job statuses to filter on. One of `ongoing`, `cancelled`, `failed`, `finished`. |
| `start_date`          | string    | no       | The start date to filter on. In unix timestamp format. Can be set without `end_date`.    |
| `end_date`            | string    | no       | The end date to filter on. In unix timestamp format. Can be set without `start_date`.    |

### Response

| Parameter                   | Type         | Description                                                                                                                    |
|-----------------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------|
| `status`                    | boolean      | True if the request was successful.                                                                                            |
| `message`                   | string       | Error message if `status` is false.                                                                                            |
| `data`                      | array        | The jobs.                                                                                                                      |
| `data[].id`                 | integer      | The job id.                                                                                                                    |
| `data[].uid`                | string       | The job uid.                                                                                                                   |
| `data[].status`             | string       | The job status. One of `ongoing`, `cancelled`, `failed`, `done`. Note that `done` is the same as `finished`                    |
| `data[].cancelReasonType`   | string       | The job cancel reason type.                                                                                                    |
| `data[].rating`             | integer      | The job rating.                                                                                                                |
| `data[].filename`           | string       | The job filename.                                                                                                              |
| `data[].startDate`          | string       | The job start date.                                                                                                            |
| `data[].endDate`            | string/null  | The job end date. Is null if the job is ongoing.                                                                               |
| `data[].user`               | integer/null | The user id of the user who started the job.                                                                                   |
| `data[].autoprint`          | boolean      | If the job was started automatically using the Auto-print feature                                                              |
| `data[].outsideSystem`      | boolean      | If the job was started outside SimplyPrint, via SD card, OctoPrint, Mailsail, Fluidd or else how                               |
| `data[].printer`            | integer      | The printer id that was used to print the job.                                                                                 |
| `data[].filament`           | string       | The filament usage. JSON encoded string with usage per extruder.                                                               |
| `data[].filUsage`           | integer      | The filament usage in mm.                                                                                                      |
| `data[].filUsageGram`       | integer      | The filament usage in grams.                                                                                                   |
| `data[].currentPercentage`  | integer      | The current percentage of the job.                                                                                             |
| `data[].cost`               | object/null  | Potential calculated cost of job.                                                                                              |
| `data[].queueItem`          | object/null  | The queue item that was used to start the job. Please note that this is only shown if you have access to view the Print Queue. |
| `data[].queueItem.id`       | integer      | The id of the queue item that was used to start the job.                                                                       |
| `data[].queueItem.user`     | integer      | The user id of the user who created the queue item.                                                                            |
| `data[].queueItem.queueNum` | integer      | The queue number of the queue item.                                                                                            |
| `page_amount`               | integer      | The total number of pages for the given parameters.                                                                            |

## Get details

```shell
curl https://api.simplyprint.io/{id}/jobs/GetDetails \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "id": "job_id"
  }'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "job": {
    "id": 123,
    "filament": [
      ...
    ],
    "pauses": [
      ...
    ],
    "currentTime": 1697040000,
    "pictures": [
      ...
    ],
    "notificationsSent": [
      ...
    ],
    "cost": 12.34,
    "customFields": {
      ...
    },
    "ended": 3600,
    "failedReason": "string",
    "cancelInfo": {
      "reason": "string",
      "comment": "string",
      "by": "string",
      "byOther": 1
    },
    "analysis": {
      ...
    },
    "notifications": {
      ...
    },
    "outsideSystem": true,
    "rating": 5,
    "started": 7200,
    "created": 10800,
    "state": "string",
    "file": "filename.gcode",
    "percentage": 50,
    "time": 1800,
    "canPreview": true,
    "layer": 10,
    "ai": [
      0.1,
      0.2,
      0.3
    ],
    "printer": {
      "id": 456,
      "name": "Printer Name",
      "extruders": 2,
      "image": "https://cdn.simplyprint.io/prints/images/printer_image.jpg",
      "deleted": 1
    },
    "spools": [
      ...
    ]
  }
}
```

`GET /{id}/jobs/GetDetails?id={job_id}`

### Request Body

| Parameter | Type   | Description  |
|-----------|--------|--------------|
| `id`      | string | The job UID. |

### Response

| Parameter               | Type    | Description                              |
|-------------------------|---------|------------------------------------------|
| `status`                | boolean | `true` if the request was successful.    |
| `message`               | string  | Error message if `status` is `false`.    |
| `job`                   | object  | Details about the print job.             |
| `job.id`                | integer | Unique identifier for the job.           |
| `job.filament`          | array   | Array of filament data.                  |
| `job.pauses`            | array   | Array of pause history.                  |
| `job.currentTime`       | integer | Current timestamp.                       |
| `job.pictures`          | array   | Array of pictures related to the job.    |
| `job.notificationsSent` | array   | Array of notifications sent.             |
| `job.cost`              | float   | Cost of the print job.                   |
| `job.customFields`      | object  | Custom fields for the job.               |
| `job.ended`             | integer | Time since the job ended.                |
| `job.failedReason`      | string  | Reason for job failure, if any.          |
| `job.cancelInfo`        | object  | Information about job cancellation.      |
| `job.analysis`          | object  | G-code analysis data.                    |
| `job.notifications`     | object  | Notification data.                       |
| `job.outsideSystem`     | boolean | `true` if the job is outside the system. |
| `job.rating`            | integer | Rating of the job.                       |
| `job.started`           | integer | Time since the job started.              |
| `job.created`           | integer | Time since the job was created.          |
| `job.state`             | string  | Current state of the job.                |
| `job.file`              | string  | Original filename of the job.            |
| `job.percentage`        | integer | Current completion percentage.           |
| `job.time`              | integer | Time left or time since the job ended.   |
| `job.canPreview`        | boolean | `true` if the job can be previewed.      |
| `job.layer`             | integer | Current layer of the print.              |
| `job.ai`                | array   | Array of AI detection values.            |
| `job.printer`           | object  | Details about the printer.               |
| `job.spools`            | array   | Array of spool data.                     |
