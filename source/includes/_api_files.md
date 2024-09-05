# API Files

The base URL for the SimplyPrint Files API is `https://files.simplyprint.io/{id}/`. Use the exact same authentication as the normal api endpoint. It is very important you ensure you send files with the correct file extension. The API will not be able to determine the file type based on the content.

## Upload a file using the API

<aside class="notice">
  This endpoint requires the <b>Print Farm</b> plan.
</aside>

`POST {id}/files/Upload`

> Example request to upload a file less than 100MB

```shell
curl -X POST "https://files.simplyprint.io/{id}/files/Upload" \
    -H 'accept: application/json' \
    -H "X-API-KEY: {API_KEY}" \
    -F "file=@/path/to/file.gcode"
```
> Success response

```json
{
    "status":true,
    "message": null,
    "file": {
        "id": "f568ead4bbc2d881efc8a9a05f3bd585334cd8c662347ba2dfad7250176b0abd",
        "name": "file.gcode",
        "size": 13439
    }
}
```

> Multiple parts for a single file larger than 100MB. The first filename is the filename of the entire file.

```shell
curl -X POST "https://files.simplyprint.io/{id}/files/Upload" \
    -H 'accept: application/json' \
    -H "X-API-KEY: {API_KEY}" \
    -F "file=@/path/to/part1.3mf"
    -F "totalSize=3352316"
```
> Success response with continueToken

```json
{
    "status":true,
    "message":null,
    "continueToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzM4NCJ9.eyJ0eXBlIjoiYWN0ao9uX3Rva2VuIiwiYWN0aW9uIjoiZmlsZV9jb250aW51ZV91cGxvYWQiLCJ1c2VyIjo2OTc2LCJjb21wYW55IjoyLCJkYXRhIja7ImJ1Y2tldEhhc2giOiI0MGQ2MzgwNmQwYWUxODhkNjc5YzY0NjA0M2RiYjUxMTc0NTViNTc1NjNlODEzZDc2MGRjMTJkMzVaYjdmY2Y0IiwidG90YWxTaXplIjoxNjc2MTU4NH0sImlhdCI6MTcyNTU2MjEzMywiZXhwIjoxNzI1NjQ4NTMzfQ.9qyNyx9A4Ox_6GrFSxXpxlpLcAKaSr8ln84X3yuWdhT_2O3L8-lGWaXAbQk9VvR-3pu1-a9p40amnt6Fghy49InjzCfNMRp-6-Ft_uMRf6PbmcCCrksvRxNP38ImoXy6"
}
```

> Continue uploading the file (send next part with only the continueToken)

```shell
curl -X POST "https://files.simplyprint.io/{id}/files/Upload" \
    -H 'accept: application/json' \
    -H "X-API-KEY: {API_KEY}" \
    -F "continueToken=eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzM4NCJ9.eyJ0eXBlIjoiYWN0ao9uX3Rva2VuIiwiYWN0aW9uIjoiZmlsZV9jb250aW51ZV91cGxvYWQiLCJ1c2VyIjo2OTc2LCJjb21wYW55IjoyLCJkYXRhIja7ImJ1Y2tldEhhc2giOiI0MGQ2MzgwNmQwYWUxODhkNjc5YzY0NjA0M2RiYjUxMTc0NTViNTc1NjNlODEzZDc2MGRjMTJkMzVaYjdmY2Y0IiwidG90YWxTaXplIjoxNjc2MTU4NH0sImlhdCI6MTcyNTU2MjEzMywiZXhwIjoxNzI1NjQ4NTMzfQ.9qyNyx9A4Ox_6GrFSxXpxlpLcAKaSr8ln84X3yuWdhT_2O3L8-lGWaXAbQk9VvR-3pu1-a9p40amnt6Fghy49InjzCfNMRp-6-Ft_uMRf6PbmcCCrksvRxNP38ImoXy6" \
    -F "file=@/path/to/part2.3mf"
```

> Sucessful final response as we ensured to only send exactly the total size of the file.

```json
{
    "status":true,
    "message":null,
    "file": {
        "id": "f568ead4bbc2d881efc8a9a05f3bd585334cd8c662347ba2dfad7250176b0abd",
        "name": "part1.3mf",
        "size": 3352316
    }
}
```

### Request

| Parameter       | Type    | Required | Description                                                                                                                                                         |
| --------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `file`             | file | yes       | Uploaded file (Max 100mb)                                                                                                      |
| `continueToken`        | string  | no       | Optional token you'll get if you need to continue the upload for files greater than 100MB.                                                                                                                           |
| `totalSize` | integer | no       | Send this if you want a continueToken, by providing the total size of the entire file you want to upload                                                                                 |

### Response

| Parameter    | Type    | Description                                                  |
| ------------ | ------- | -------------------------------------------------------------| 
| `status`     | boolean | True if the request was successful.                          |
| `message`    | string  | Error message if `status` is false.                          |
| `continueToken` | string   | For every subsequent request that still has some pending size based on the total size this will be returned instead of the file.                                   |
| `file.*`        | object    | Final file object after entire file has been uploaded    | 
| `file.id`      | string   | The API File ID you'll need to use other SimplyPrint APIs |
| `file.name`  | string  | Name used to reference the file |
| `file.size`       | int   | Total size of uploaded file |

