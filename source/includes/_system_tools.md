# System Tools

The System Admin Tools endpoints (cron monitor, database health and data cleanup) require the `fst_manage_settings` permission. Actions that change data (`cron/run`, `database/repair`, `DELETE cleanup`) additionally require a site administrator (`manage_options`). Read endpoints return `can_manage` so a client knows whether the actions are available.

## REST Method Test

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/rest-test' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command checks that the HTTP method reaches the REST API

```json
{
    "message": "ok"
}
```

This endpoint accepts any HTTP method (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) and always returns `ok`. The admin UI uses it to check that the server or firewall does not block any method.

### HTTP Request

`ANY https://yourdomain.com/wp-json/fluent-support/v2/system-tools/rest-test`

## Get Cron Jobs

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cron' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the scheduled tasks

```json
{
    "can_manage": true,
    "server": {
        "memory_limit": "256 MB",
        "usage_percent": 16.41,
        "max_execution_time": "30 seconds",
        "has_server_cron": false
    },
    "events": [
        {
            "hook": "fluent_support_hourly_tasks",
            "title": "Hourly Cleanup and Auto-Close",
            "interval": 3600,
            "next_run_human": "in 25 mins",
            "status": "ok"
        },
        {
            "hook": "fluent_support_daily_tasks",
            "title": "Daily Log Cleanup",
            "interval": 86400,
            "next_run_human": "in 5 hours",
            "status": "ok"
        },
        {
            "hook": "fluent_support_weekly_tasks",
            "title": "Weekly Maintenance",
            "interval": 604800,
            "next_run_human": "2 weeks ago",
            "status": "overdue"
        }
    ]
}
```

This endpoint returns Fluent Support's scheduled tasks and the server limits they run under. `status` is `ok`, `overdue` (missed by more than one interval) or `not_scheduled`. With Pro, `fluent_support_half_hourly` (Email Piping Retry and Storage Tokens) is listed too. `has_server_cron` is `true` when `DISABLE_WP_CRON` is set.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cron`

## Run Cron Job

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cron/run' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command runs a scheduled task now

```json
{
    "message": "The task ran successfully."
}
```

This endpoint runs one of the listed tasks immediately. A hook that is not in the list returns the error `This task can not be run manually.` Requires a site administrator.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cron/run`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
hook | string | yes | The task hook, e.g. `fluent_support_hourly_tasks`, `fluent_support_daily_tasks`, `fluent_support_weekly_tasks`

## Get Database Health

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/database' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the status of every Fluent Support table

```json
{
    "can_manage": true,
    "tables": [
        {
            "name": "fs_tickets",
            "title": "Tickets",
            "status": "ok"
        },
        {
            "name": "fs_conversations",
            "title": "Ticket Replies and Notes",
            "status": "ok"
        },
        {
            "name": "fs_persons",
            "title": "Customers and Agents",
            "status": "ok"
        }
    ]
}
```

This endpoint checks whether each Fluent Support table exists. `status` is `ok` or `missing`; missing tables are listed first. Tables created only when their feature is first used (AI activity logs, internal notifications) are skipped when absent. Table names are shown without the WordPress table prefix.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/system-tools/database`

## Repair Database

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/database/repair' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command repairs the database tables

```json
{
    "message": "Repair finished."
}
```

This endpoint re-runs the database migrations, which create missing tables and add missing columns. Existing data is not changed. Requires a site administrator.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/system-tools/database/repair`

## Get Cleanup Counts

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cleanup?days_before=30&selected_logs[]=activity_logs&selected_logs[]=ai_activity_logs' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command counts old log rows

```json
{
    "can_manage": true,
    "log_counts": [
        {
            "key": "activity_logs",
            "title": "Activity Logs",
            "count": 1520
        },
        {
            "key": "ai_activity_logs",
            "title": "AI Activity Logs",
            "count": 42
        }
    ]
}
```

This endpoint counts the log rows older than `days_before` days for each selected log type. At least one valid type and 7 or more days are required (otherwise `Select at least one log type and 7 days or more.`).

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cleanup`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
days_before | int | yes | Count rows older than this many days (minimum 7)
selected_logs[] | array | yes | Log types: `activity_logs`, `ai_activity_logs`, `internal_notifications` (add-ons may register more)

## Delete Old Logs

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cleanup?days_before=30&selected_logs[]=activity_logs' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command deletes old log rows

```json
{
    "deleted": 10000,
    "has_more": true,
    "message": "Old logs deleted."
}
```

This endpoint deletes log rows older than `days_before` days, up to 10,000 rows per call. Repeat the call while `has_more` is `true`. Requires a site administrator.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/system-tools/cleanup`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
days_before | int | yes | Delete rows older than this many days (minimum 7)
selected_logs[] | array | yes | Log types: `activity_logs`, `ai_activity_logs`, `internal_notifications` (add-ons may register more)
