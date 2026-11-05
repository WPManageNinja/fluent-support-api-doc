# Notifications

Notifications are the in-app notifications of the current agent. The requesting user must be a support agent; otherwise the error `Sorry, You do not have permission. Please add yourself as support agent first` is returned. Only notifications for tickets the agent can still access are returned. While notifications are turned off (the notification tables do not exist), lists are empty and counts are 0.

## Get Notifications

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/notifications?per_page=15&page=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the agent's notifications.

```json
{
    "notifications": {
        "current_page": 1,
        "data": [
            {
                "id": 211,
                "actor_id": 85,
                "ticket_id": 193,
                "conversation_id": 1450,
                "event_type": "customer_replied",
                "category": "ticket_activity",
                "payload": {
                    "ticket_title": "Unable to activate license",
                    "conversation_type": "response",
                    "content_preview": "I have tried again but it still fails..."
                },
                "actor": {
                    "id": 85,
                    "full_name": "Giulio Nieder",
                    "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
                },
                "ticket": {
                    "id": 193,
                    "title": "Unable to activate license"
                },
                "recipients": [
                    {
                        "id": 530,
                        "user_id": 26,
                        "is_read": 0,
                        "read_at": null
                    }
                ],
                "created_at": "2021-12-06 09:31:02",
                "updated_at": "2021-12-06 09:31:02",
                "event_label": "replied to a ticket",
                "event_icon": "reply",
                "summary": "Giulio Nieder replied to a ticket in Unable to activate license"
            }
        ],
        "first_page_url": "/wp-json/fluent-support/v2/notifications?page=1",
        "from": 1,
        "last_page": 4,
        "last_page_url": "/wp-json/fluent-support/v2/notifications?page=4",
        "next_page_url": "/wp-json/fluent-support/v2/notifications?page=2",
        "path": "/wp-json/fluent-support/v2/notifications",
        "per_page": 15,
        "prev_page_url": null,
        "to": 15,
        "total": 52
    },
    "unread_count": 7,
    "meta": {
        "categories": [
            {"key": "all", "label": "All"},
            {"key": "mentions", "label": "Mentions"},
            {"key": "ticket_activity", "label": "Ticket Activity"},
            {"key": "automation_triggers", "label": "Automation Triggers"}
        ]
    },
    "notification_categories": [
        {"key": "all", "label": "All"},
        {"key": "mentions", "label": "Mentions"},
        {"key": "ticket_activity", "label": "Ticket Activity"},
        {"key": "automation_triggers", "label": "Automation Triggers"}
    ]
}
```

This endpoint returns the current agent's notifications, newest first, paginated, together with the total unread count and the notification categories. `event_type` is one of `agent_mentioned`, `agent_replied`, `ticket_assigned`, `ticket_reassigned`, `customer_replied`, `ticket_closed`, `ticket_reopened`, `workflow_triggered`.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/notifications`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Notifications per page, default 15, max 100
page | int | no | Page number, default 1
status | string | no | `read` or `unread`
category | string | no | `all`, `mentions`, `ticket_activity` or `automation_triggers`
ticket_id | int | no | Only notifications of this ticket

## Get Unread Notifications

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/notifications/unread?limit=8' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the latest unread notifications.

```json
{
    "notifications": [
        {
            "id": 211,
            "actor_id": 26,
            "ticket_id": 193,
            "conversation_id": 1452,
            "event_type": "agent_mentioned",
            "category": "mentions",
            "payload": {
                "ticket_title": "Unable to activate license",
                "conversation_type": "note"
            },
            "actor": {
                "id": 26,
                "full_name": "Michael Smith",
                "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
            },
            "ticket": {
                "id": 193,
                "title": "Unable to activate license"
            },
            "recipients": [
                {
                    "id": 532,
                    "user_id": 37,
                    "is_read": 0,
                    "read_at": null
                }
            ],
            "created_at": "2021-12-06 10:02:11",
            "updated_at": "2021-12-06 10:02:11",
            "event_label": "mentioned you",
            "event_icon": "reply",
            "summary": "Michael Smith mentioned you in Unable to activate license"
        }
    ],
    "unread_count": 7,
    "meta": {
        "categories": [
            {"key": "all", "label": "All"},
            {"key": "mentions", "label": "Mentions"},
            {"key": "ticket_activity", "label": "Ticket Activity"},
            {"key": "automation_triggers", "label": "Automation Triggers"}
        ]
    },
    "notification_categories": [
        {"key": "all", "label": "All"},
        {"key": "mentions", "label": "Mentions"},
        {"key": "ticket_activity", "label": "Ticket Activity"},
        {"key": "automation_triggers", "label": "Automation Triggers"}
    ]
}
```

This endpoint returns the latest unread notifications of the current agent (not paginated), with the total unread count and the notification categories.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/notifications/unread`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
limit | int | no | Number of notifications to return, default 8, max 20
category | string | no | `all`, `mentions`, `ticket_activity` or `automation_triggers`
ticket_id | int | no | Only notifications of this ticket

## Get Unread Notification Count

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/notifications/unread-count' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the unread notification count.

```json
{
    "count": 7
}
```

This endpoint returns the number of unread notifications of the current agent.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/notifications/unread-count`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
category | string | no | `all`, `mentions`, `ticket_activity` or `automation_triggers`
ticket_id | int | no | Only count notifications of this ticket

## Mark All Notifications as Read

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/notifications/mark-all-read' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command marks all notifications as read.

```json
{
    "updated": 7,
    "unread_count": 0
}
```

This endpoint marks all unread notifications of the current agent as read. `updated` is the number of notifications changed.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/notifications/mark-all-read`

## Mark Notification as Read

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/notifications/211/mark-read' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command marks a notification as read.

```json
{
    "updated": 1,
    "unread_count": 6
}
```

This endpoint marks one notification of the current agent as read. `updated` is `0` when the notification was already read or does not belong to the agent.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/notifications/{notification_id}/mark-read`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
notification_id | int | yes | Notification id
