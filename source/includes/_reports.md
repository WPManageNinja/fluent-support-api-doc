# Reports

## Get Overall Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/stats?date_range[]=2021-12-01&date_range[]=2021-12-31' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns overall ticket stats.

```json
{
    "overall_reports": {
        "new_tickets": {
            "title": "New Tickets",
            "key": "new_tickets",
            "count": 109
        },
        "active_tickets": {
            "title": "Active Tickets",
            "key": "active_tickets",
            "count": 11
        },
        "closed_tickets": {
            "title": "Closed Tickets",
            "key": "closed_tickets",
            "count": 11
        },
        "responses": {
            "title": "Responses",
            "key": "responses",
            "count": 68
        },
        "open_tickets": {
            "title": "Open Tickets",
            "key": "open_tickets",
            "count": 120
        }
    }
}
```

This endpoint returns the overall ticket counts (new, active, closed, responses and open tickets) for a date range. Without a date range every ticket is counted. Counts only include tickets in mailboxes the current agent can access. When `agent_id` is given, `active_tickets` is omitted and `responses` counts only the replies written by that agent (titled "Total Responses"); otherwise it counts both agent and customer replies. Closed tickets are counted by their resolved date. Requires the `fst_view_all_reports` permission.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/stats`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | date (YYYY-MM-DD) | no | Filter by date range, like date_range[]=2021-12-01&date_range[]=2021-12-31 (a comma separated `date_range=2021-12-01,2021-12-31` also works)
mailbox_id | int | no | Only count tickets of this mailbox (`business_box` is accepted as an alias)
product_id | int | no | Only count tickets of this product
agent_id | int | no | Only count tickets assigned to this agent
customer_id | int | no | Only count tickets of this customer

## Get Agents Summary

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/agents-summary?from=2021-12-01&to=2021-12-31' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns agents summary.

```json
{
    "summary": [
        {
            "id": 13,
            "first_name": "Nicholas",
            "last_name": "Rose",
            "stats": {
                "interactions": 7,
                "responses": 21,
                "opens": 11,
                "closed": 3,
                "waiting_tickets": 0
            },
            "active_stat": {
                "average_waiting": "1 day",
                "max_waiting": "3 days",
                "waiting_tickets": 2
            },
            "full_name": "Nicholas Rose",
            "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
        },
        {
            "id": 26,
            "first_name": "Michael ",
            "last_name": "Smith",
            "stats": {
                "interactions": 17,
                "responses": 26,
                "opens": 50,
                "closed": 4,
                "waiting_tickets": 0
            },
            "active_stat": {
                "average_waiting": "4 days",
                "max_waiting": "2 weeks",
                "waiting_tickets": 5
            },
            "full_name": "Michael  Smith",
            "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
        },
        {
            "id": 37,
            "first_name": "John",
            "last_name": "Murphy",
            "stats": {
                "interactions": 20,
                "responses": 17,
                "opens": 1,
                "closed": 8,
                "waiting_tickets": 0
            },
            "active_stat": {
                "average_waiting": 0,
                "max_waiting": 0,
                "waiting_tickets": 0
            },
            "full_name": "John Murphy",
            "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
        }
    ]
}
```

This endpoint returns one summary row per agent: tickets closed, responses and interactions (distinct tickets replied to) in the date range (default today), currently open tickets (`opens`), and current waiting stats (`active_stat`). When agent feedback is enabled, each `stats` object also contains `likes` and `dislikes`. Requires the `fst_view_all_reports` permission.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/agents-summary`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | no | Start date of the summary, default today
to| date (YYYY-MM-DD) | no | End date of the summary, default today

## Get Day & Time Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/day-time-stats?report_type=ticket&date_range[]=2021-12-01&date_range[]=2021-12-31' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns counts grouped by weekday and hour.

```json
{
    "stats": {
        "Mon": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0},
        "Tue": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0},
        "Wed": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0},
        "Thu": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0},
        "Fri": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0},
        "Sat": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0},
        "Sun": {"0:00": 0, "1:00": 0, "2:00": 0, "3:00": 0, "4:00": 0, "5:00": 0, "6:00": 0, "7:00": 0, "8:00": 5, "9:00": 3, "10:00": 1, "11:00": 8, "12:00": 6, "13:00": 4, "14:00": 2, "15:00": 0, "16:00": 7, "17:00": 5, "18:00": 3, "19:00": 1, "20:00": 8, "21:00": 0, "22:00": 0, "23:00": 0}
    }
}
```

This endpoint returns new tickets, agent responses or customer responses counted by weekday and hour of the day (site time). An unknown `report_type` returns an empty `stats` array. Requires the `fst_view_all_reports` permission.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/day-time-stats`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
report_type | string | yes | What to count: `ticket`, `agent_response` or `customer_response`
date_range[] | date (YYYY-MM-DD) | no | Filter by date range, like date_range[]=2021-12-01&date_range[]=2021-12-31. Without both dates all records are counted
agent_id | int | no | Only used with `report_type=agent_response`: count only this agent's responses

## Get Mailbox Ticket Growth

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailbox-reports/tickets-growth?mailbox_id=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns created tickets per period.

```json
{
    "stats": {
        "2021-11-06": 34,
        "2021-11-07": 38,
        "2021-11-08": 26,
        "2021-11-09": 37,
        "2021-11-10": 28,
        "2021-11-11": 21,
        "2021-11-12": 20,
        "2021-11-13": 22,
        "2021-11-14": 32,
        "2021-11-15": 36,
        "2021-11-16": 27,
        "2021-11-17": 18,
        "2021-11-18": 31,
        "2021-11-19": 42,
        "2021-11-20": 33,
        "2021-11-21": 17,
        "2021-11-22": 29,
        "2021-11-23": 30,
        "2021-11-24": 19,
        "2021-11-25": 40,
        "2021-11-26": 41,
        "2021-11-27": 30,
        "2021-11-28": 50,
        "2021-11-29": 52,
        "2021-11-30": 43,
        "2021-12-01": 51,
        "2021-12-02": 20,
        "2021-12-03": 29,
        "2021-12-04": 42,
        "2021-12-05": 30,
        "2021-12-06": 25
    }
}
```

This endpoint returns the number of tickets created per period, for one mailbox or for all mailboxes. The default range is the last 30 days. The period depends on the range: hourly (`"09:00 - 09:59"` keys) for a single day, daily up to 62 days, weekly (key is the week's first day) up to 181 days, and monthly (`"Dec 2021"` keys) beyond that. Requires the `fst_sensitive_data` permission.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/mailbox-reports/tickets-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mailbox_id | int | no | Only count tickets of this mailbox. Leave empty for all mailboxes
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31

## Get Mailbox Ticket Resolve Growth

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailbox-reports/tickets-resolve-growth?mailbox_id=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns closed tickets per period.

```json
{
    "stats": {
        "2021-11-06": 34,
        "2021-11-07": 38,
        "2021-11-08": 26,
        "2021-11-09": 37,
        "2021-11-10": 28,
        "2021-11-11": 21,
        "2021-11-12": 20,
        "2021-11-13": 22,
        "2021-11-14": 32,
        "2021-11-15": 36,
        "2021-11-16": 27,
        "2021-11-17": 18,
        "2021-11-18": 31,
        "2021-11-19": 42,
        "2021-11-20": 33,
        "2021-11-21": 17,
        "2021-11-22": 29,
        "2021-11-23": 30,
        "2021-11-24": 19,
        "2021-11-25": 40,
        "2021-11-26": 41,
        "2021-11-27": 30,
        "2021-11-28": 50,
        "2021-11-29": 52,
        "2021-11-30": 43,
        "2021-12-01": 51,
        "2021-12-02": 20,
        "2021-12-03": 29,
        "2021-12-04": 42,
        "2021-12-05": 30,
        "2021-12-06": 25
    }
}
```

This endpoint returns the number of tickets closed (by resolved date) per period, for one mailbox or for all mailboxes. Periods and the default range work like the ticket growth endpoint. Requires the `fst_sensitive_data` permission.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/mailbox-reports/tickets-resolve-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mailbox_id | int | no | Only count tickets of this mailbox. Leave empty for all mailboxes
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31

## Get Mailbox Response Growth

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailbox-reports/response-growth?mailbox_id=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the stats of total replies by agents.

```json
{
    "stats": {
        "2021-11-06": 34,
        "2021-11-07": 38,
        "2021-11-08": 26,
        "2021-11-09": 37,
        "2021-11-10": 28,
        "2021-11-11": 21,
        "2021-11-12": 20,
        "2021-11-13": 22,
        "2021-11-14": 32,
        "2021-11-15": 36,
        "2021-11-16": 27,
        "2021-11-17": 18,
        "2021-11-18": 31,
        "2021-11-19": 42,
        "2021-11-20": 33,
        "2021-11-21": 17,
        "2021-11-22": 29,
        "2021-11-23": 30,
        "2021-11-24": 19,
        "2021-11-25": 40,
        "2021-11-26": 41,
        "2021-11-27": 30,
        "2021-11-28": 50,
        "2021-11-29": 52,
        "2021-11-30": 43,
        "2021-12-01": 51,
        "2021-12-02": 20,
        "2021-12-03": 29,
        "2021-12-04": 42,
        "2021-12-05": 30,
        "2021-12-06": 25
    }
}
```

This endpoint returns the number of agent responses per period, for one mailbox or for all mailboxes. Periods and the default range work like the ticket growth endpoint. Requires the `fst_sensitive_data` permission.
<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/mailbox-reports/response-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
mailbox_id | int | no | Only count responses on tickets of this mailbox. Leave empty for all mailboxes
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31
