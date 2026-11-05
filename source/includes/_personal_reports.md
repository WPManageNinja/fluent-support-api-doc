# Get Personal Reports

## Get Ticket Resolve Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/my-reports/tickets-resolve-growth' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns total ticket closed by user.

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

This endpoint returns the number of tickets assigned to the current agent that were closed (by resolved date) per period. The default range is the last 30 days. The period depends on the range: hourly for a single day, daily up to 62 days, weekly up to 181 days and monthly beyond that.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/my-reports/tickets-resolve-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31

## Get Ticket Response Growth

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/my-reports/response-growth' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns total replies done by user.

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

This endpoint returns the number of responses written by the current agent per period. Periods and the default range work like the ticket resolve stats endpoint.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/my-reports/response-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31


## Get User Summary

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/my-reports/my-summary' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns user's summary.

```json
{
    "summary": [
        {
            "id": 26,
            "first_name": "Michael ",
            "last_name": "Smith",
            "stats": {
                "interactions": 18,
                "responses": 30,
                "opens": 50,
                "closed": 6,
                "waiting_tickets": 0
            },
            "active_stat": {
                "average_waiting": "4 days",
                "max_waiting": "2 weeks",
                "waiting_tickets": 42
            },
            "full_name": "Michael  Smith",
            "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
        }
    ]
}
```

This endpoint returns the summary of the current agent: tickets closed, responses and interactions in the date range (default today), currently open tickets (`opens`) and current waiting stats (`active_stat`). When agent feedback is enabled (Pro), `stats` also contains `likes` and `dislikes`.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/my-reports/my-summary`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | no | Start date of the summary, default today
to| date (YYYY-MM-DD) | no | End date of the summary, default today

## Get My Dashboard Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/my_stats?with[]=overall_stats&with[]=tickets_by_products' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the current agent's dashboard stats.

```json
{
    "stats": {
        "new_tickets": {
            "title": "New Tickets",
            "count": 4
        },
        "active_tickets": {
            "title": "Active Tickets",
            "count": 12
        },
        "closed_tickets": {
            "title": "Closed Tickets",
            "count": 3
        },
        "responses": {
            "title": "Total Responses",
            "count": 17
        },
        "interactions": {
            "title": "Interactions",
            "count": 9
        },
        "unassigned_tickets": {
            "title": "Unassigned Tickets",
            "count": 5
        }
    },
    "overall_stats": {
        "average_waiting": "6 hours",
        "max_waiting": "3 days",
        "waiting_tickets": 14
    },
    "tickets_by_product": {
        "1": {
            "title": "Fluent Support",
            "count": 8
        },
        "2": {
            "title": "Fluent Forms",
            "count": 3
        }
    },
    "dashboard_notice": ""
}
```

This endpoint returns the dashboard cards of the current agent: their new and active tickets, plus the tickets they closed, the responses they wrote and the tickets they replied to (interactions) today. `unassigned_tickets` is only included when the agent has the `fst_manage_unassigned_tickets` permission. Extra widgets are added with the `with` parameter: `suggested_tickets` (up to 5 tickets waiting on the agent), `ticket_to_watch` (Pro only), `overall_stats` (waiting stats of all accessible tickets) and `tickets_by_products` (tickets awaiting a reply per product, returned as `tickets_by_product`). With Pro active, `dashboard_notice` is always included.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/my_stats`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
with[] | array | no | Extra widgets to include: `suggested_tickets`, `ticket_to_watch`, `overall_stats`, `tickets_by_products`

## Get Agent Performance

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/agent_performance' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns today's performance of each agent.

```json
{
    "agent_today_stats": [
        {
            "agent_id": 26,
            "agent_name": "Michael Smith",
            "agent_email": "michael@example.com",
            "avatar": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128",
            "stats": {
                "waiting_today": {
                    "title": "Waiting Today",
                    "count": 6
                },
                "new_tickets": {
                    "title": "New Tickets",
                    "count": 2
                },
                "closed_tickets": {
                    "title": "Closed Tickets",
                    "count": 3
                },
                "responses": {
                    "title": "Responses",
                    "count": 14
                }
            }
        }
    ]
}
```

This endpoint returns today's numbers for every agent who replied today or has tickets awaiting a reply: tickets awaiting a reply, tickets created and closed today, and responses written today. Agents without the `fst_agent_today_performance` permission get an empty `agent_today_stats` array.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/agent_performance`
