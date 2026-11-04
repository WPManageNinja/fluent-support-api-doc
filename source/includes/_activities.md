# Activities

## Get Activities

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/activity-logger' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns activities

```json
{
    "activities": {
        "total": 189,
        "per_page": 15,
        "current_page": 1,
        "last_page": 19,
        "next_page_url": "/wp-json/fluent-support/v2/activity-logger?page=2",
        "prev_page_url": null,
        "from": 1,
        "to": 10,
        "data": [
            {
                "id": 507,
                "person_id": "85",
                "person_type": "customer",
                "event_type": "fluent_support/ticket_created",
                "object_id": "193",
                "object_type": "ticket",
                "description": "<a class=\"fs_link_trans fs_pr\" href=\"#view_customer\">Giulio Nieder</a> created a <a class=\"fs_link_trans fs_tk\" href=\"#view_ticket\">Ticket: Hello, I'm considering FluentSupport. I tried the free version, and t looks l... (#168)</a> via email",
                "created_at": "2021-12-06 09:31:02",
                "updated_at": "2021-12-06 09:31:02",
                "person": {
                    "first_name": "Giulio",
                    "person_type": "customer",
                    "last_name": "Nieder",
                    "id": 85,
                    "avatar": null,
                    "full_name": "Giulio Nieder",
                    "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
                }
            }
        ]
    },
    "settings": {
        "delete_days": 14,
        "disable_logs": "no",
        "open_link_in_new_tab": "no"
    }
}
```

This endpoint returns all activities, newest first, with the activity settings. Requires the `fst_view_activity_logs` permission.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/activity-logger`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Activities to show per page, default 15, max 150
page | int | no | Activity page number, default 1
from | date (YYYY-MM-DD) | no | Start date, used together with `to`
to | date (YYYY-MM-DD) | no | End date, used together with `from`
filters[agent_id] | int | no | Only show activities by this person

## Get Activity Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns activity settings.

```json
{
    "activity_settings": {
        "delete_days": 14,
        "disable_logs": "no",
        "open_link_in_new_tab": "no"
    }
}
```

This endpoint returns activity settings. Requires the `fst_manage_settings` permission.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings`

## Update Activity Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'activity_settings[delete_days]="14"' \
--form 'activity_settings[disable_logs]="no"'
```

> The above command updates activity settings.

```json
{
    "message": "Activity settings have been updated"
}
```

This endpoint updates activity settings. Fields that are not sent are reset to their defaults. Requires the `fst_manage_settings` permission.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
activity_settings[delete_days] | int | no | Delete activities automatically & you can define after how many days you want to delete this, default 14
activity_settings[disable_logs] | text | no | This takes yes/no value, use yes to disable log, default no
activity_settings[open_link_in_new_tab] | text | no | This takes yes/no value, use yes to open activity links in a new tab, default no

## Get AI Activities

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ai-activity-logger' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns AI activities.

```json
{
    "data": [
        {
            "id": 42,
            "agent_id": 26,
            "ticket_id": 193,
            "model_name": "gpt-4o-mini",
            "tokens": 812,
            "prompt": "Summarize this ticket",
            "created_at": "2021-12-06 09:45:10",
            "updated_at": "2021-12-06 09:45:10",
            "person": {
                "first_name": "Michael",
                "person_type": "agent",
                "last_name": "Smith",
                "id": 26,
                "avatar": null,
                "full_name": "Michael Smith",
                "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
            },
            "ticket": {
                "id": 193,
                "title": "Unable to activate license"
            }
        }
    ],
    "total": 57,
    "per_page": 10,
    "current_page": 1,
    "last_page": 6,
    "settings": {
        "delete_days": 14,
        "disable_logs": "no"
    }
}
```

This endpoint returns the AI activity log (AI requests made by agents, with the model, tokens used and prompt), newest first, with the AI activity settings. If the AI activity log table does not exist yet, an empty list is returned. Requires the `fst_view_activity_logs` permission.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/ai-activity-logger`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Activities to show per page, default 10
page | int | no | Activity page number, default 1
from | date (YYYY-MM-DD) | no | Start date, used together with `to`
to | date (YYYY-MM-DD) | no | End date, used together with `from`
filters[agent_id] | int | no | Only show activities of this agent

## Get AI Activity Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ai-activity-logger/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns AI activity settings.

```json
{
    "ai_activity_settings": {
        "delete_days": 14,
        "disable_logs": "no"
    }
}
```

This endpoint returns AI activity settings. Requires the `fst_manage_settings` permission.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ai-activity-logger/settings`

## Update AI Activity Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ai-activity-logger/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'ai_activity_settings[delete_days]="14"' \
--form 'ai_activity_settings[disable_logs]="no"'
```

> The above command updates AI activity settings.

```json
{
    "message": "AI Activity settings have been updated"
}
```

This endpoint updates AI activity settings. Send both fields: a missing `delete_days` is saved as `0` and a missing `disable_logs` as an empty value. Requires the `fst_manage_settings` permission.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ai-activity-logger/settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ai_activity_settings[delete_days] | int | yes | Delete AI activities automatically after this many days
ai_activity_settings[disable_logs] | text | yes | This takes yes/no value, use yes to disable AI activity logging
