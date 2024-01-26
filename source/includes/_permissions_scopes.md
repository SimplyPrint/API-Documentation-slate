# Permissions and Scopes

## Company permissions

These permissions are used to control access within a company. The permissions are tied to the user groups within the company.

The owner of a company has all permissions.

| Permission               | Description                                                     |
| ------------------------ | --------------------------------------------------------------- |
| `PRINTER_EDIT`           | "Allow to create, edit and delete printers"                     |
| `CREATE_FILAMENT`        | "Allow creating, editing and deleting filament spools"          |
| `CHANGE_FILAMENT`        | "Allow change of filament"                                      |
| `PRINT_QUEUE_REMOVE_ALL` | "Allow deletion of other user's print queue items"              |
| `ORG_RANK_MANAGEMENT`    | "Allow to create, edit, delete and change order of user groups" |
| `VIEW_USERS`             | "Allow overview of users in organization"                       |
| `INVITE_USERS`           | "Allow invite of other users"                                   |
| `DELETE_USER`            | "Allow delete users"                                            |
| `SLICER_ORG_PROFILES`    | "Allow to create and edit organisation slicer profiles"         |
| `EDIT_TAGS`              | "Allow user to create, update and delete tags"                  |

## OAuth2 scopes

These scopes are used to control access to the API. The scopes are tied to the OAuth2 access token.

| Scope                | Description                               |
| -------------------- | ----------------------------------------- |
| `user.read`          | View user details                         |
| `printers.read`      | View printer details                      |
| `printers.write`     | Manage details of printers                |
| `printers.actions`   | Perform actions on printers               |
| `spools.read`        | View filament spools                      |
| `spools.write`       | Manage filament spools                    |
| `files.read`         | View file and folder details and contents |
| `files.write`        | Manage files and folders                  |
| `files.temp_upload`  | Upload temporary files                    |
| `queue.read`         | View print queue                          |
| `queue.write`        | Manage print queue                        |
| `statistics.read`    | View printing statistics                  |
| `print_history.read` | View print history                        |
| `slicer.read`        | View slicer profiles                      |
| `slicer.write`       | Manage slicer profiles                    |
| `tags.read`          | View tags                                 |
| `tags.write`         | Manage tags                               |
