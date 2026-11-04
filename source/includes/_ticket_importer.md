# Ticket Importer

Import tickets, replies and customers from other helpdesks. The endpoints require the `fst_manage_settings` permission.

Supported sources (`handler`):

Handler | Source | Type
------- | ------ | ----
`awesome-support` | Awesome Support (WordPress plugin) | Available only while the plugin is active
`support-candy` | SupportCandy (WordPress plugin) | Available only while the plugin is active
`js-helpdesk` | JS Help Desk (WordPress plugin) | Available only while the plugin is active
`helpscout` | Help Scout | Cloud service (OAuth access token)
`freshdesk` | Freshdesk | Cloud service (domain + API key)
`zendesk` | Zendesk | Cloud service (domain + email + API token)

Add-ons can register more handlers with the `fluent_support/migrator_class_mapper` filter.

## Get Import Sources

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the available import sources

```json
{
    "stats": [
        {
            "name": "Awesome Support",
            "tickets": 120,
            "replies": 340,
            "customers": 85,
            "handler": "awesome-support",
            "last_migrated": false
        },
        {
            "name": "Zendesk",
            "handler": "zendesk",
            "type": "sass",
            "last_migrated": false,
            "previously_imported": {
                "domain": ""
            }
        }
    ]
}
```

This endpoint returns the sources that can be imported on this site with their stats. WordPress plugin sources report their ticket, reply and customer counts; cloud sources report the progress of a previous, unfinished import (`previously_imported`) so it can be resumed.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer`

## Import Tickets

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer/import' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command imports one page of tickets

```json
{
    "handler": "awesome-support",
    "insert_ids": [101, 102, 103],
    "skips": 0,
    "has_more": true,
    "completed": 15,
    "imported_page": 1,
    "total_pages": 7,
    "next_page": 2,
    "total_tickets": 130,
    "remaining": 110
}
```

This endpoint imports one page of tickets (20 by default, filterable with `fluent_support/ticket_import_chunk_limit`). Call it repeatedly with `page` set to `next_page` until `has_more` is `false`; the last page includes `message` (`All tickets have been imported successfully`). Tickets that were already imported are skipped. `completed` is the percentage done for the WordPress plugin sources, Help Scout and Zendesk. For Freshdesk, `completed` is the number of tickets imported on that page, `total_pages` and `total_tickets` are `null`, and `remaining` is `0`. Zendesk responses also include `imported_tickets`, `cursor`, `next_page_url` and `include_archived`; pass `cursor` back in `query[cursor]`. If there is nothing to import, the response is `message` with `had_tickets: no`. An unknown handler returns `Unknown ticket importer.`

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer/import`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
handler | string | yes | The source, see the table above
page | int | yes | Page to import, starting at 1
query[access_token] | string | for cloud sources | Help Scout access token, Freshdesk API key or Zendesk API token
query[mailbox] | int | for `helpscout` | The Help Scout mailbox ID to import
query[domain] | string | for `freshdesk`, `zendesk` | Your Freshdesk/Zendesk URL, must be `https://` (e.g. `https://yourcompany.freshdesk.com`)
query[email] | string | for `zendesk` | The Zendesk agent email the API token belongs to
query[cursor] | string | no | Zendesk: the `cursor` from the previous response
query[include_archived] | boolean | no | Zendesk: also import archived tickets

## Get Help Scout Access Token

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer/helpscout/token' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command exchanges a Help Scout authorization code for an access token

```json
{
    "access_token": "HELPSCOUT_ACCESS_TOKEN",
    "refresh_token": "HELPSCOUT_REFRESH_TOKEN",
    "token_type": "bearer",
    "expires_in": 172800
}
```

This endpoint exchanges a Help Scout OAuth authorization code for an access token, server side, so the App Secret is never put in a browser URL. Use the returned `access_token` as `query[access_token]` when importing. Missing values return `App ID, App Secret and Authorization Code are required.`; a Help Scout error is passed back as the error message.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer/helpscout/token`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
app_id | string | yes | Help Scout App ID
app_secret | string | yes | Help Scout App Secret
code | string | yes | The authorization code from Help Scout

## Delete Source Tickets

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer/delete?handler=awesome-support&page=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command deletes one page of the source plugin's tickets

```json
{
    "has_more": true,
    "next_page": 2
}
```

This endpoint permanently deletes the source plugin's own tickets after an import, one page per call. Call it with `next_page` until `has_more` is `false`; the last response includes `message` (`All tickets have been deleted successfully`). Only WordPress plugin sources (`awesome-support`, `support-candy`, `js-helpdesk`) delete anything; cloud sources return an empty response. `support-candy` and `js-helpdesk` delete all of the plugin's data in a single call (`has_more: false`) by truncating all of the plugin's tables.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/ticket_importer/delete`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
handler | string | yes | `awesome-support`, `support-candy` or `js-helpdesk`
page | int | yes | Page to delete, starting at 1
