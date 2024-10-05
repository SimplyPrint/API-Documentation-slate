# Permissions and Scopes

## Company permissions

These permissions are used to control access within a company. The permissions are tied to the user groups within the
company.

The owner of a company has all permissions.

| Permission                       | Description                                                                                          |
|----------------------------------|------------------------------------------------------------------------------------------------------|
| `view_news`                      | "Allow viewing news in the application"                                                              |
| `org_admin`                      | "Allow administrating organization settings and permissions"                                         |
| `panel_printing`                 | "Allow access to the printing panel"                                                                 |
| `printer_restart`                | "Allow restarting printers"                                                                          |
| `printer_edit`                   | "Allow creating, editing, and deleting printers"                                                     |
| `bed_leveling`                   | "Allow performing bed leveling on printers"                                                          |
| `gcode_profiles`                 | "Allow managing GCode macros and snippets"                                                           |
| `printer_settings`               | "Allow configuring printer settings"                                                                 |
| `filament_settings`              | "Allow managing filament settings"                                                                   |
| `change_filament`                | "Allow changing filament on printers"                                                                |
| `create_filament`                | "Allow creating, editing, and deleting filament spools"                                              |
| `see_filament_tab`               | "Allow viewing the filament tab"                                                                     |
| `view_users`                     | "Allow viewing user overview in the organization"                                                    |
| `change_user_rank`               | "Allow changing users' group (rank) within the organization"                                         |
| `assign_peer_rank`               | "Allow assigning other users' group (rank) to the same rank as yours"                                |
| `manual_user_email_confirm`      | "Allow manually confirming user email addresses"                                                     |
| `invite_users`                   | "Allow inviting other users to the organization"                                                     |
| `delete_user`                    | "Allow deleting users from the organization"                                                         |
| `org_user_registration_settings` | "Allow managing user registration settings for the organization"                                     |
| `org_hub_settings`               | "Allow managing hub settings for the organization"                                                   |
| `org_rank_management`            | "Allow creating, editing, deleting, and reordering user groups (ranks)"                              |
| `org_view_statistics`            | "Allow viewing organization-wide statistics (for all users)"                                         |
| `refill_quota`                   | "Allow refilling user quota (coming soon)"                                                           |
| `custom_slicer_profiles`         | "Allow creating and deleting custom slicer profiles"                                                 |
| `org_profiles`                   | "Allow creating and editing organization slicer profiles"                                            |
| `all_slicer_modes`               | "Allow accessing all slicer modes"                                                                   |
| `see_slicer_default_profiles`    | "Allow viewing default slicer profiles"                                                              |
| `queue_remove_all`               | "Allow deleting other users' print queue items"                                                      |
| `org_api`                        | "Allow users' API key to be used with this organization"                                             |
| `create_org_folder`              | "Allow creating organization (shared) folders"                                                       |
| `cancel_others`                  | "Allow canceling other users' print jobs"                                                            |
| `see_who_printed`                | "Allow viewing who printed specific items in various popups ("Print removed", "Cancel print", etc.)" |
| `edit_tags`                      | "Allow creating, updating, and deleting custom tags"                                                 |
| `goto_local`                     | "Allow going to local printer UI (OctoPrint, Mainsail, Fluidd, etc.)"                                |
| `printer_info`                   | "Allow viewing extra printer information ("Printer info" popup)"                                     |
| `send_raw_gcode`                 | "Allow sending raw GCode commands to printers"                                                       |
| `widget_device_health`           | "Allow access to device health widget"                                                               |
| `widget_control`                 | "Allow access to printer control widget"                                                             |
| `change_temps`                   | "Allow changing printer temperatures"                                                                |
| `change_print_speed`             | "Allow changing the print speed"                                                                     |
| `printer_delete`                 | "Allow deleting printers"                                                                            |
| `printer_add`                    | "Allow adding new printers"                                                                          |
| `print_queue`                    | "Allow access to the print queue"                                                                    |
| `reorder_queue`                  | "Allow reordering print queue items"                                                                 |
| `see_cam`                        | "Allow viewing printer camera feeds"                                                                 |
| `clear_bed`                      | "Allow clearing the print bed"                                                                       |
| `slice`                          | "Allow slicing models for printing"                                                                  |
| `print`                          | "Allow starting print jobs"                                                                          |
| `pause`                          | "Allow pausing print jobs"                                                                           |
| `cancel`                         | "Allow canceling print jobs"                                                                         |
| `queue_groups`                   | "Allow managing print queue groups"                                                                  |
| `queue_edit_others`              | "Allow editing other users' print queue items"                                                       |
| `queue_read_notes`               | "Allow reading notes on other users' print queue items"                                              |
| `queue_download_others`          | "Allow downloading files from other users' print queue items"                                        |
| `queue_see_done_items`           | "Allow viewing completed print queue items"                                                          |
| `queue_revive_done_items`        | "Allow reviving completed print queue items"                                                         |
| `queue_assign_printers`          | "Allow assigning printers to print queue items"                                                      |
| `queue_print_slice`              | "Allow printing directly from sliced items in the queue"                                             |
| `queue_see_others`               | "Allow viewing other users' print queue items"                                                       |
| `files_nozzle_tag`               | "Allow managing nozzle tags for files"                                                               |
| `files_material_tag`             | "Allow managing material tags for files"                                                             |
| `files_assign_custom_tags`       | "Allow assigning custom tags to files"                                                               |
| `queue_nozzle_tag`               | "Allow managing nozzle tags for queue items"                                                         |
| `queue_material_tag`             | "Allow managing material tags for queue items"                                                       |
| `queue_assign_custom_tags`       | "Allow assigning custom tags to queue items"                                                         |
| `can_export`                     | "Allow exporting data (CSV export)"                                                                  |
| `autoprint_manage`               | "Allow managing AutoPrint settings"                                                                  |
| `courses_manage`                 | "Allow managing courses"                                                                             |
| `courses_view`                   | "Allow viewing courses"                                                                              |
| `webhooks_manage`                | "Allow managing webhooks"                                                                            |
| `set_user_teacher`               | "Allow setting a user as a teacher ("School" plan only)"                                             |
| `change_user_school_class`       | "Allow changing user's school class ("School" plan only)"                                            |
| `org_school_settings_manage`     | "Allow managing school settings for the organization ("School" plan only)"                           |
| `access_all_printers`            | "Allow access to all printers in the organization"                                                   |
| `custom_fields_manage`           | "Allow creating and deleting custom fields"                                                          |

## OAuth2 scopes

These scopes are used to control access to the API. The scopes are tied to the OAuth2 access token.

| Scope                | Description                               |
|----------------------|-------------------------------------------|
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
