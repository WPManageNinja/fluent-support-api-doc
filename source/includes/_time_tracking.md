# Time Tracking

## Get time entries of a ticket

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/time-tracks/<ticket_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "tracks": [
        {
            "id": 12,
            "ticket_id": 138,
            "created_at": "2021-12-03 17:20:11",
            "updated_at": "2021-12-03 17:20:11",
            "completed_at": "2021-12-03 17:20:11",
            "working_minutes": 45,
            "billable_minutes": 45,
            "message": "Debugged the SMTP settings",
            "is_manual": true,
            "ticket": null,
            "agent": {
                "id": 1,
                "first_name": "John",
                "last_name": "Doe",
                "full_name": "John Doe",
                "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
                "user_id": 1
            },
            "customer": null
        }
    ],
    "estimated_minutes": 120
}
```

This endpoint returns the time entries logged on a ticket (newest first) and the ticket's estimated time in minutes (`null` when no estimate is set).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/time-tracks/<ticket_id>`

## Log time on a ticket

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/time-tracks/<ticket_id>?working_minutes=45&message=Debugged the SMTP settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "track": {
        "id": 12,
        "ticket_id": 138,
        "created_at": "2021-12-03 17:20:11",
        "updated_at": "2021-12-03 17:20:11",
        "completed_at": "2021-12-03 17:20:11",
        "working_minutes": 45,
        "billable_minutes": 45,
        "message": "Debugged the SMTP settings",
        "is_manual": true,
        "ticket": null,
        "agent": {
            "id": 1,
            "first_name": "John",
            "last_name": "Doe",
            "full_name": "John Doe",
            "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
            "user_id": 1
        },
        "customer": null
    },
    "message": "You have successfully submitted your working time"
}
```

This endpoint records time that the current agent worked on a ticket. The billable minutes are set to the working minutes.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/time-tracks/<ticket_id>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
working_minutes | integer | yes | Minutes worked. Must be greater than 0.
message | text | no | A note about the work done (HTML allowed).

## Set estimated time of a ticket

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/time-tracks/<ticket_id>/estimated-time?estimated_minutes=120' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Estimated time has been updated"
}
```

This endpoint sets the estimated time of a ticket.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/time-tracks/<ticket_id>/estimated-time`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
estimated_minutes | integer | yes | The estimated time in minutes.

## Timesheet by tickets

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/timesheet/by-tickets?date_range[]=2021-12-01&date_range[]=2021-12-07&mailbox_id[]=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "tickets": [
        {
            "id": 138,
            "title": "About Features",
            "slug": "about-features"
        }
    ],
    "mail_boxes": [
        {
            "id": 1,
            "name": "Support",
            "box_type": "web"
        }
    ],
    "time_sheets": {
        "2021-12-03": {
            "138": [
                {
                    "id": 12,
                    "ticket_id": 138,
                    "created_at": "2021-12-03 17:20:11",
                    "updated_at": "2021-12-03 17:20:11",
                    "completed_at": "2021-12-03 17:20:11",
                    "working_minutes": 45,
                    "billable_minutes": 45,
                    "message": "Debugged the SMTP settings",
                    "is_manual": true,
                    "ticket": {
                        "id": 138,
                        "title": "About Features",
                        "slug": "about-features"
                    },
                    "agent": {
                        "id": 1,
                        "first_name": "John",
                        "last_name": "Doe",
                        "full_name": "John Doe",
                        "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
                        "user_id": 1
                    },
                    "customer": null
                }
            ]
        }
    },
    "total_minutes": 45,
    "date_labels": [
        "2021-12-01",
        "2021-12-02",
        "2021-12-03",
        "2021-12-04",
        "2021-12-05",
        "2021-12-06",
        "2021-12-07"
    ],
    "date_range": [
        "2021-12-01 00:00:00",
        "2021-12-07 23:59:59"
    ]
}
```

This endpoint returns the logged time in a date range, grouped by day and by ticket. `time_sheets` is keyed by date, then by ticket ID. `total_minutes` is the sum of billable minutes. Only tickets in the mailboxes the current agent can access are counted.

<aside class="notice">This endpoint requires Fluent Support Pro and the reports permission.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/timesheet/by-tickets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | array | no | Start and end date (`YYYY-MM-DD`). Defaults to the last week. At most 31 days from the start date.
mailbox_id[] | array | no | Only count tickets in these mailbox IDs.

## Timesheet by agents

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/timesheet/by-agents?date_range[]=2021-12-01&date_range[]=2021-12-07&agent_id[]=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "agents": [
        {
            "id": 1,
            "first_name": "John",
            "last_name": "Doe",
            "full_name": "John Doe",
            "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
            "user_id": 1
        }
    ],
    "all_agents": [
        {
            "id": 1,
            "first_name": "John",
            "last_name": "Doe"
        }
    ],
    "time_sheets": {
        "2021-12-03": {
            "1": [
                {
                    "id": 12,
                    "ticket_id": 138,
                    "completed_at": "2021-12-03 17:20:11",
                    "working_minutes": 45,
                    "billable_minutes": 45,
                    "message": "Debugged the SMTP settings",
                    "is_manual": true,
                    "ticket": {
                        "id": 138,
                        "title": "About Features",
                        "slug": "about-features"
                    }
                }
            ]
        }
    },
    "total_minutes": 45,
    "date_labels": ["2021-12-01", "2021-12-02", "2021-12-03", "2021-12-04", "2021-12-05", "2021-12-06", "2021-12-07"],
    "date_range": ["2021-12-01 00:00:00", "2021-12-07 23:59:59"]
}
```

This endpoint returns the logged time in a date range, grouped by day and by agent. `time_sheets` is keyed by date, then by agent ID. Each entry has the same fields as in [Timesheet by tickets](#timesheet-by-tickets). `all_agents` lists every agent, for the filter.

<aside class="notice">This endpoint requires Fluent Support Pro and the reports permission.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/timesheet/by-agents`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | array | no | Start and end date (`YYYY-MM-DD`). Defaults to the last week. At most 31 days from the start date.
agent_id[] | array | no | Only count time logged by these agent IDs.

## Timesheet by customers

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/timesheet/by-customers?date_range[]=2021-12-01&date_range[]=2021-12-07&customer_id[]=61' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "customers": [
        {
            "id": 61,
            "first_name": "Sarah",
            "last_name": "Smith",
            "full_name": "Sarah Smith",
            "photo": "https://www.gravatar.com/avatar/1e6fd8e56879c84999cd481255530592?s=128",
            "user_id": 0
        }
    ],
    "all_customers": [
        {
            "id": 61,
            "first_name": "Sarah",
            "last_name": "Smith"
        }
    ],
    "time_sheets": {
        "2021-12-03": {
            "61": [
                {
                    "id": 12,
                    "ticket_id": 138,
                    "completed_at": "2021-12-03 17:20:11",
                    "working_minutes": 45,
                    "billable_minutes": 45,
                    "message": "Debugged the SMTP settings",
                    "is_manual": true,
                    "ticket": {
                        "id": 138,
                        "title": "About Features",
                        "slug": "about-features"
                    }
                }
            ]
        }
    },
    "total_minutes": 45,
    "date_labels": ["2021-12-01", "2021-12-02", "2021-12-03", "2021-12-04", "2021-12-05", "2021-12-06", "2021-12-07"],
    "date_range": ["2021-12-01 00:00:00", "2021-12-07 23:59:59"]
}
```

This endpoint returns the logged time in a date range, grouped by day and by customer. `time_sheets` is keyed by date, then by customer ID. `all_customers` lists only customers that have time entries, for the filter.

<aside class="notice">This endpoint requires Fluent Support Pro and the reports permission.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/timesheet/by-customers`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | array | no | Start and end date (`YYYY-MM-DD`). Defaults to the last week. At most 31 days from the start date.
customer_id[] | array | no | Only count time logged on tickets of these customer IDs.
