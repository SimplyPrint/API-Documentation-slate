# Schools

## List classes

`GET /{id}/account/settings/school/classes/GetClasses`

> Example request

```shell
curl https://api.simplyprint.io/{id}/account/settings/school/classes/GetClasses \
-H 'accept: application/json' \
-H 'X-API-KEY: {API_KEY}'
```

> Example response

```json
{
  "status": true,
  "message": null,
  "objects": {
    "classes": [
      {
        "id": 1,
        "name": "Class A",
        "endMonthDay": "06-30",
        "fullStartDate": "2024-09-01",
        "fullEndDate": "2025-06-30",
        "nextClass": 2,
        "deleteUserAfterClassEnd": false,
        "autoNextClass": true,
        "googleClassroomLink": "https://classroom.google.com/c/...",
        "ssoGroupId": "group_id",
        "lastSsoSync": "2024-09-01T12:00:00Z",
        "updatedAt": "2024-09-07T08:30:00Z",
        "createdAt": "2024-08-30T08:30:00Z",
        "sortPosition": 1
      }
    ],
    "settings": {
      "defaultClass": 1,
      "maxClasses": 5,
      "userCanAddClasses": true
    }
  }
}
```

### Response

| Parameter                                   | Type    | Description                                                   |
|---------------------------------------------|---------|---------------------------------------------------------------|
| `status`                                    | boolean | True if the request was successful.                           |
| `message`                                   | string  | Error message if `status` is false.                           |
| `objects`                                   | object  | The main object containing the response data.                 |
| `objects.classes`                           | array   | An array of classes.                                          |
| `objects.classes[].id`                      | integer | The unique ID of the class.                                   |
| `objects.classes[].name`                    | string  | The name of the class.                                        |
| `objects.classes[].endMonthDay`             | string  | The end month and day of the class (MM-DD format).            |
| `objects.classes[].fullStartDate`           | string  | The full start date of the class (YYYY-MM-DD format).         |
| `objects.classes[].fullEndDate`             | string  | The full end date of the class (YYYY-MM-DD format).           |
| `objects.classes[].nextClass`               | integer | The ID of the next class, if any.                             |
| `objects.classes[].deleteUserAfterClassEnd` | boolean | Whether to delete the user after the class ends.              |
| `objects.classes[].autoNextClass`           | boolean | Whether to automatically enroll in the next class.            |
| `objects.classes[].googleClassroomLink`     | string  | The Google Classroom link associated with the class.          |
| `objects.classes[].ssoGroupId`              | string  | The SSO group ID associated with the class.                   |
| `objects.classes[].lastSsoSync`             | string  | The timestamp of the last SSO sync, or null if never synced.  |
| `objects.classes[].updatedAt`               | string  | The timestamp of the last update (UTC).                       |
| `objects.classes[].createdAt`               | string  | The timestamp of when the class was created (UTC).            |
| `objects.classes[].sortPosition`            | integer | The sort position of the class.                               |
| `objects.settings`                          | object  | The settings related to school classes.                       |
| `objects.settings.defaultClass`             | integer | The ID of the default class.                                  |
| `objects.settings.maxClasses`               | integer | The maximum number of classes per user, or null if unlimited. |
| `objects.settings.userCanAddClasses`        | boolean | Whether the user has permission to add new classes.           |

## Create or update a class

`POST /{id}/account/settings/school/classes/SaveClass`

> Example Request

```json
{
  "id": 1,
  "name": "Physics 101",
  "endMonthDay": "12-31",
  "fullStartDate": "2024-01-01",
  "fullEndDate": "2024-12-31",
  "autoNextClass": true,
  "nextClass": 2,
  "deleteUserAfterClassEnd": false,
  "googleClassroomLink": "https://classroom.google.com/c/abc123",
  "ssoGroupId": "group1"
}
```

### Request Parameters

| Parameter                 | Type    | Required | Description                                                                                                          |
|---------------------------|---------|----------|----------------------------------------------------------------------------------------------------------------------|
| `id`                      | integer | No       | The unique ID of the class. If provided, the endpoint updates the existing class; otherwise, a new class is created. |
| `name`                    | string  | Yes      | The name of the class (maximum length: 16 characters).                                                               |
| `endMonthDay`             | string  | No       | The end month and day of the class in `MM-DD` format.                                                                |
| `fullStartDate`           | string  | No       | The full start date of the class in `YYYY-MM-DD` format.                                                             |
| `fullEndDate`             | string  | No       | The full end date of the class in `YYYY-MM-DD` format.                                                               |
| `autoNextClass`           | boolean | No       | Indicates if users should be automatically enrolled in the next class.                                               |
| `nextClass`               | integer | No       | The ID of the next class, if `autoNextClass` is `true`.                                                              |
| `deleteUserAfterClassEnd` | boolean | No       | Indicates if the user should be deleted after the class ends.                                                        |
| `googleClassroomLink`     | string  | No       | The URL to the Google Classroom link associated with the class. Must be a valid HTTPS URL.                           |
| `ssoGroupId`              | string  | No       | The SSO group ID associated with the class.                                                                          |

### Response

| Parameter                               | Type    | Description                                                                         |
|-----------------------------------------|---------|-------------------------------------------------------------------------------------|
| `objects`                               | object  | The main object containing the response data.                                       |
| `objects.class`                         | object  | The saved class object.                                                             |
| `objects.class.id`                      | integer | The unique ID of the class.                                                         |
| `objects.class.name`                    | string  | The name of the class.                                                              |
| `objects.class.endMonthDay`             | string  | The end month and day of the class (MM-DD format).                                  |
| `objects.class.fullStartDate`           | string  | The full start date of the class (YYYY-MM-DD format).                               |
| `objects.class.fullEndDate`             | string  | The full end date of the class (YYYY-MM-DD format).                                 |
| `objects.class.autoNextClass`           | boolean | Indicates if users are automatically enrolled in the next class.                    |
| `objects.class.nextClass`               | integer | The ID of the next class, if applicable.                                            |
| `objects.class.deleteUserAfterClassEnd` | boolean | Indicates if users should be deleted after the class ends.                          |
| `objects.class.googleClassroomLink`     | string  | The Google Classroom link associated with the class.                                |
| `objects.class.ssoGroupId`              | string  | The SSO group ID associated with the class.                                         |
| `objects.class.lastSsoSync`             | string  | The date and time of the last SSO synchronization in UTC format, or `null` if none. |
| `objects.class.updatedAt`               | string  | The date and time when the class was last updated in UTC format, or `null` if none. |
| `objects.class.createdAt`               | string  | The date and time when the class was created in UTC format, or `null` if none.      |
| `objects.class.sortPosition`            | integer | The sort position of the class.                                                     |
