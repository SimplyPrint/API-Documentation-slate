# Courses

<aside class="notice">
  It is not currently possible to create courses via the API. Contact us if this is a feature you would like to see.
</aside>

## Get course

```shell
curl https://api.simplyprint.io/{id}/courses/GetCourse?id=123 \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "course": {
    // Course details
  },
  "slides": [
    // Course slides
  ],
  "embeds": [
    {
      "name": "Embed",
      "logo": "example.png",
      "privacy_policy": "https://example.com/privacy",
      "allowed_url": "https://example.com/embed",
      "how_to_embed_guide": "https://example.com/embed_guide"
    }
  ]
}
```

| Required permission | Description                |
|---------------------|----------------------------|
| `courses_view`      | Required to view courses.  |
| `courses_manage`    | Required for teacher view. |

`GET /{id}/courses/GetCourse`

### Request Parameters

| Parameter      | Type    | Description                                          |
|----------------|---------|------------------------------------------------------|
| `id`           | integer | The ID of the course to retrieve.                    |
| `teacher_view` | boolean | Whether to retrieve the course in teacher view mode. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
| `course`  | object  | The course details.                   |
| `slides`  | array   | The slides of the course.             |
| `embeds`  | array   | The embed types for the course.       |

## Delete course

```shell
curl -X POST https://api.simplyprint.io/{id}/courses/DeleteCourse \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "id": 123
  }'
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
| `courses_manage`     |

`POST /{id}/courses/DeleteCourse`

### Request Body

| Parameter | Type    | Description                     |
|-----------|---------|---------------------------------|
| `id`      | integer | The ID of the course to delete. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Move course

```shell
curl -X POST https://api.simplyprint.io/{id}/courses/MoveCourse \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "course": 123,
    "category": 456
  }'
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
| `courses_manage`     |

`POST /{id}courses/MoveCourse`

### Request Body

| Parameter  | Type    | Description                   |
|------------|---------|-------------------------------|
| `course`   | integer | The ID of the course to move. |
| `category` | integer | The ID of the new category.   |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Publish/unpublish course

```shell
curl -X POST https://api.simplyprint.io/{id}/courses/PublishCourse \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "id": 123
  }'
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
| `courses_manage`     |

`POST /{id}/courses/PublishCourse`

### Request Parameters

| Parameter   | Description                                                  |
|-------------|--------------------------------------------------------------|
| `unpublish` | Set if the course should be unpublished (no value required). |

### Request Body

| Parameter | Type    | Description                      |
|-----------|---------|----------------------------------|
| `id`      | integer | The ID of the course to publish. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |

## Create/update category

```shell
curl -X POST https://api.simplyprint.io/{id}/courses/CreateCategory \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "name": "New Category",
    "public": true
  }'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "category": {
    "id": 123,
    "name": "New Category",
    "public": true
  }
}
```

| Required permissions |
|----------------------|
| `courses_manage`     |

`POST /{id}/courses/CreateCategory`

### Request Body

| Parameter  | Type    | Description                                                              |
|------------|---------|--------------------------------------------------------------------------|
| `language` | string  | The language code (2 characters) (optional).                             |
| `id`       | integer | The ID of the category to update (optional to update existing category). |
| `name`     | string  | The name of the category.                                                |
| `public`   | boolean | Whether the category is public.                                          |

### Response

| Parameter  | Type    | Description                              |
|------------|---------|------------------------------------------|
| `status`   | boolean | `true` if the request was successful.    |
| `message`  | string  | Error message if `status` is `false`.    |
| `category` | object  | The created or updated category details. |

## Delete category

```shell
curl -X POST https://api.simplyprint.io/{id}/courses/DeleteCategory \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}' \
  -d '{
    "id": 123
  }'
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
| `courses_manage`     |

`POST /{id}/courses/DeleteCategory`

### Request Body

| Parameter | Type    | Description                       |
|-----------|---------|-----------------------------------|
| `id`      | integer | The ID of the category to delete. |

### Response

| Parameter | Type    | Description                           |
|-----------|---------|---------------------------------------|
| `status`  | boolean | `true` if the request was successful. |
| `message` | string  | Error message if `status` is `false`. |
