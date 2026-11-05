# Advanced Reports

All Advanced Reports endpoints require Fluent Support Pro and both the `fst_view_all_reports` and `fst_sensitive_data` permissions. Counts only include tickets in mailboxes the current agent can access. Endpoints that take `from` and `to` require both dates (YYYY-MM-DD, both days included); `from` must not be after `to` and the range cannot exceed 1 year, otherwise a validation error is returned (for example `from and to must be valid dates in YYYY-MM-DD format`).

## Get Ticket Overview

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/ticket-overview?from=2021-12-01&to=2021-12-03' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the ticket overview report.

```json
{
    "workload": {
        "awaiting_reply": 14,
        "avg_wait": "6 hours",
        "unassigned": 5
    },
    "tickets": {
        "new": 42,
        "not_responded": 7,
        "closed": 31
    },
    "agent_responses": {
        "total_responses": 96,
        "tickets_handled": 38,
        "active_agents": 4,
        "avg_per_agent": 24
    },
    "same_day_response": {
        "same_day": 30,
        "not_same_day": 12
    },
    "activity": {
        "granularity": "day",
        "labels": ["2021-12-01", "2021-12-02", "2021-12-03"],
        "ticket_creation": [15, 12, 15],
        "agent_replies": [30, 35, 31]
    },
    "product_distribution": {
        "total": 42,
        "products": [
            {
                "id": 1,
                "name": "Fluent Support",
                "tickets": 25,
                "percent": 100
            },
            {
                "id": 0,
                "name": "No Product",
                "tickets": 10,
                "percent": 40
            },
            {
                "id": 2,
                "name": "Fluent Forms",
                "tickets": 7,
                "percent": 28
            }
        ]
    },
    "products": [
        {
            "id": 2,
            "name": "Fluent Forms"
        },
        {
            "id": 1,
            "name": "Fluent Support"
        }
    ]
}
```

This endpoint returns the Ticket Overview report: current workload (`workload`), tickets created, not yet responded and closed in the range, agent response totals, how many new tickets got a first agent reply on the same day, a tickets/replies activity chart (hourly with 24 `"12 AM"`...`"11 PM"` labels when `from` equals `to`, otherwise daily), the top 5 products by tickets created (`percent` is relative to the top product) and the list of all products.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/ticket-overview`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date of the report
to | date (YYYY-MM-DD) | yes | End date of the report
product_id | int | no | Only count tickets of this product

## Get Response Activity

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/response-activity?from=2021-12-01&to=2021-12-03' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns tickets created and agent replies per day.

```json
{
    "granularity": "day",
    "labels": ["2021-12-01", "2021-12-02", "2021-12-03"],
    "ticket_creation": [15, 12, 15],
    "agent_replies": [30, 35, 31]
}
```

This endpoint returns the number of tickets created and agent replies per day of the range. When `from` equals `to`, `granularity` is `hour` and the arrays hold 24 values labelled `"12 AM"` to `"11 PM"`.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/response-activity`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date of the report
to | date (YYYY-MM-DD) | yes | End date of the report

## Get Product Insights

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/product-insights?from=2021-12-01&to=2021-12-31' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns per product insights.

```json
{
    "products": [
        {
            "id": 2,
            "name": "Fluent Forms",
            "volume": 18,
            "responses": 40,
            "unanswered": 2,
            "waiting": 3,
            "interactions": 15,
            "opens": 6,
            "closed": 12
        },
        {
            "id": 1,
            "name": "Fluent Support",
            "volume": 25,
            "responses": 61,
            "unanswered": 4,
            "waiting": 5,
            "interactions": 22,
            "opens": 9,
            "closed": 17
        },
        {
            "id": 0,
            "name": "No Product",
            "volume": 10,
            "responses": 12,
            "unanswered": 3,
            "waiting": 2,
            "interactions": 7,
            "opens": 4,
            "closed": 6
        }
    ]
}
```

This endpoint returns one row per product (and a `No Product` row with id `0` when it has data): tickets created in the range (`volume`), how many of those are still open (`opens`) or have no response (`unanswered`), tickets closed in the range, agent responses and tickets replied to (`interactions`) in the range, and tickets waiting for a reply now (`waiting`).
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/product-insights`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date of the report
to | date (YYYY-MM-DD) | yes | End date of the report

## Get Workload Health

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/workload-health' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the current workload.

```json
{
    "all": {
        "awaiting_reply": 14,
        "avg_wait": "6 hours",
        "unassigned": 5
    },
    "mailboxes": [
        {
            "id": 1,
            "name": "Support",
            "awaiting_reply": 12,
            "avg_wait": "5 hours",
            "unassigned": 4
        },
        {
            "id": 2,
            "name": "Sales",
            "awaiting_reply": 2,
            "avg_wait": "2 days",
            "unassigned": 1
        }
    ],
    "waiting_tickets": [
        {
            "id": 193,
            "subject": "Unable to activate license",
            "customer": "Giulio Nieder",
            "waiting_since": "2021-12-03 09:31:02",
            "product": "Fluent Support",
            "mailbox_id": 1
        }
    ]
}
```

This endpoint returns the workload right now (it takes no date range): tickets waiting for an agent reply, their average wait and unassigned open tickets, overall (`all`) and per accessible mailbox, plus the 10 tickets that have waited the longest.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/workload-health`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mailbox_id | int | no | Only list the longest waiting tickets of this mailbox (`all` and `mailboxes` are not affected)

## Get Performance Overview Filters

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/performance-overview/filters' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns agents and teams to filter by.

```json
{
    "agents": [
        {
            "id": 37,
            "name": "John Murphy"
        },
        {
            "id": 26,
            "name": "Michael Smith"
        }
    ],
    "teams": [
        {
            "id": 3,
            "name": "Billing Team",
            "agent_ids": [26, 37]
        }
    ]
}
```

This endpoint returns the agents and agent groups (teams, with their member agent ids) that can be used with the performance overview endpoint.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/performance-overview/filters`

## Get Performance Overview

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/performance-overview?from=2021-12-01&to=2021-12-02&agent_ids=26,37' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the performance of the selected agents.

```json
{
    "agents": [
        {
            "id": 26,
            "name": "Michael Smith",
            "responses": 21,
            "first_response": "2021-12-01 08:12:40",
            "last_response": "2021-12-02 17:45:03",
            "avg_per_day": 10.5
        },
        {
            "id": 37,
            "name": "John Murphy",
            "responses": 0,
            "first_response": null,
            "last_response": null,
            "avg_per_day": 0
        }
    ],
    "activity": {
        "granularity": "day",
        "labels": ["2021-12-01", "2021-12-02"],
        "series": [
            {
                "agent_id": 26,
                "agent_name": "Michael Smith",
                "data": [9, 12]
            },
            {
                "agent_id": 37,
                "agent_name": "John Murphy",
                "data": [0, 0]
            }
        ]
    },
    "tickets": {
        "data": [
            {
                "id": 193,
                "subject": "Unable to activate license",
                "customer": "Giulio Nieder",
                "agent": "Michael Smith",
                "responses": 3,
                "last_reply": "2021-12-02 17:45:03",
                "status": "active",
                "status_label": "Active",
                "status_variant": "warning"
            }
        ],
        "current_page": 1,
        "last_page": 3,
        "total": 14
    },
    "totals": {
        "closed": 8,
        "open": 5
    },
    "growth": {
        "tickets": {
            "2021-12-01": 3,
            "2021-12-02": 4
        },
        "resolved": {
            "2021-12-01": 5,
            "2021-12-02": 3
        },
        "responses": {
            "2021-12-01": 9,
            "2021-12-02": 12
        }
    }
}
```

This endpoint returns the performance of the selected agents in the range: responses per agent (with first/last response time and average per day), a per-agent activity chart (hourly when `from` equals `to`), the tickets they replied to (paged, newest first), closed and open totals of their assigned tickets, and growth charts (tickets created, resolved, responses). Without `agent_ids` an empty report is returned.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/performance-overview`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date of the report
to | date (YYYY-MM-DD) | yes | End date of the report
agent_ids | string | no | Comma separated agent ids, like `26,37`
page | int | no | Page of the replied tickets list, default 1
per_page | int | no | Replied tickets per page, default 6, max 50

## Get Report Snapshots

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/snapshots?report_type=ticket_overview' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the snapshots of a report.

```json
{
    "snapshots": [
        {
            "id": 58,
            "snapshot_time": "2021-12-06 12:00:00"
        },
        {
            "id": 53,
            "snapshot_time": "2021-12-06 06:00:00"
        }
    ]
}
```

This endpoint lists the saved snapshots (id and time, newest first) of one report. Snapshots hold site-wide data, so agents with mailbox restrictions always get an empty list. An unknown `report_type` returns the error `Invalid report type.`
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/snapshots`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
report_type | string | yes | One of `ticket_overview`, `performance_overview`, `workload_health`, `product_insights`, `response_activity`

## Get Report Snapshot

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/snapshots/58' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns a single snapshot.

```json
{
    "id": 58,
    "snapshot_time": "2021-12-06 12:00:00",
    "report_type": "workload_health",
    "data": {
        "all": {
            "awaiting_reply": 14,
            "avg_wait": "6 hours",
            "unassigned": 5
        },
        "mailboxes": [],
        "waiting_tickets": []
    }
}
```

This endpoint returns one snapshot with its saved report `data`. The `data` has the same shape as the matching live report endpoint, plus `cumulative_range` and/or `chart_range` (`{"from": ..., "to": ...}`) describing the ranges used; `response_activity` snapshots also include an `activity_grid` with `ticket`, `agent_response` and `customer_response` weekday/hour grids. Agents with mailbox restrictions get a forbidden error; an unknown id returns `Snapshot not found.`
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/snapshots/{id}`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
id | int | yes | Snapshot id

## Run Ticket Audit

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/audit/run' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'from="2021-12-01"' \
--form 'to="2021-12-31"'
```

> The above command returns the tickets queued for an AI audit.

```json
{
    "total": 48,
    "queued": 48,
    "ticket_ids": [193, 191, 188, 185]
}
```

This endpoint starts an AI Ticket Audit: it returns the open tickets created in the range that need an audit (never audited, failed, or with a customer reply since the last audit), newest first. `total` is the number of eligible tickets and `ticket_ids` holds at most 200 of them (filterable with `fluent_support/ai_audit_batch_limit`). Send the ids to the process batch endpoint in chunks. An AI provider must be configured under Settings → AI Integration, otherwise a validation error explains what is missing.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/audit/run`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date (ticket creation date)
to | date (YYYY-MM-DD) | yes | End date (ticket creation date)

## Process Ticket Audit Batch

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/audit/process-batch' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'from="2021-12-01"' \
--form 'to="2021-12-31"' \
--form 'ticket_ids[]="193"' \
--form 'ticket_ids[]="191"'
```

> The above command audits a batch of tickets.

```json
{
    "results": [
        {
            "ticket_id": 193,
            "title": "Unable to activate license",
            "product": "Fluent Support",
            "agent": "Michael Smith",
            "mood": "Frustrated",
            "score": 3.5,
            "summary": "The customer cannot activate their license after several attempts.",
            "status": "completed",
            "error": null,
            "audited_at": "2021-12-06 10:15:22",
            "stale": false
        },
        {
            "ticket_id": 191,
            "title": "Export entries to CSV",
            "product": "Fluent Forms",
            "agent": null,
            "mood": null,
            "score": null,
            "summary": null,
            "status": "failed",
            "error": "Could not parse the AI response.",
            "audited_at": "2021-12-06 10:15:25",
            "stale": false
        }
    ],
    "submitted": 2,
    "eligible": 2
}
```

This endpoint audits up to 5 of the given tickets with the configured AI provider and saves the result (mood, a 0-10 score and a one-line summary). Ids that are not eligible for the current agent and range are skipped. `mood` is one of `Happy`, `Neutral`, `Frustrated`, `Very Unhappy`. Each user can make 100 AI audit calls per hour; beyond that a `429` error `AI audit limit reached for this hour. Please try again later.` is returned. A missing `ticket_ids` returns `ticket_ids is required`.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/audit/process-batch`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date, same as used for the run endpoint
to | date (YYYY-MM-DD) | yes | End date, same as used for the run endpoint
ticket_ids[] | array | yes | Ticket ids to audit; only the first 5 are processed

## Get Ticket Audit Results

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/audit/results?from=2021-12-01&to=2021-12-31' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the saved audit results.

```json
{
    "rows": {
        "data": [
            {
                "ticket_id": 193,
                "title": "Unable to activate license",
                "product": "Fluent Support",
                "agent": "Michael Smith",
                "mood": "Frustrated",
                "score": "3.5",
                "summary": "The customer cannot activate their license after several attempts.",
                "status": "completed",
                "error": null,
                "audited_at": "2021-12-06 10:15:22",
                "stale": false
            }
        ],
        "current_page": 1,
        "last_page": 3,
        "per_page": 20,
        "total": 48
    },
    "summary": {
        "total": 48,
        "success": 46,
        "failed": 2,
        "average_score": 6.8,
        "needs_attention": 9
    },
    "distribution": {
        "Happy": 20,
        "Neutral": 17,
        "Frustrated": 7,
        "Very Unhappy": 2
    },
    "needs_attention_rows": [
        {
            "ticket_id": 188,
            "title": "Refund request",
            "mood": "Very Unhappy",
            "score": "1.0"
        }
    ]
}
```

This endpoint returns the saved audits of open tickets created in the range, newest first. `stale` is true when the customer replied after the audit. Unless `with_aggregates` is false, the response also includes a `summary`, the mood `distribution` and the 3 lowest scoring `Frustrated` / `Very Unhappy` tickets (`needs_attention_rows`).
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/advanced-reports/audit/results`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | yes | Start date (ticket creation date)
to | date (YYYY-MM-DD) | yes | End date (ticket creation date)
page | int | no | Page number, default 1
per_page | int | no | Results per page, default 20, max 150
mood_filter | string | no | `all` (default), `failed`, or one mood: `Happy`, `Neutral`, `Frustrated`, `Very Unhappy`
with_aggregates | boolean | no | Set to false to skip `summary`, `distribution` and `needs_attention_rows`, default true
