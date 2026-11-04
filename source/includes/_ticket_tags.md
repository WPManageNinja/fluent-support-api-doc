# Ticket Tags

<aside class="notice">All ticket tag endpoints require Fluent Support Pro.</aside>

## Get Tags

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command will return all tags

```json
{
    "tags": {
        "current_page": 1,
        "data": [
            {
                "id": 1,
                "tag_type": "ticket_tag",
                "title": "New Customer",
                "slug": "nw-customer",
                "description": "",
                "settings": "",
                "created_by": "1",
                "created_at": "2021-12-01 13:53:20",
                "updated_at": "2021-12-09 12:42:15",
                "tickets_count": 12,
                "count": 12
            }
        ],
        "per_page": 15,
        "total": 1,
        "last_page": 1
    }
}
```

This endpoint will return all tags (paginated, newest first) with the number of tickets of each tag.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Records per page (default 15)
page | int | no | Page number for pagination (default 1)
search | string | no | Search tags by title or description

## Get Tag Options

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/options' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return all tags as options

```json
{
    "option_key": "ticket_tags",
    "options": [
        {
            "id": 2,
            "title": "Bug"
        },
        {
            "id": 1,
            "title": "New Customer"
        }
    ]
}
```

This endpoint will return all tags as `id`/`title` pairs, sorted by title (used for tag selectors).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/options`

## Get a Specific Tag

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific tag by id

```json
{
    "tags": {
        "id": 1,
        "tag_type": "ticket_tag",
        "title": "New Customer",
        "slug": "nw-customer",
        "description": "",
        "settings": "",
        "created_by": "1",
        "created_at": "2021-12-01 13:53:20",
        "updated_at": "2021-12-09 12:42:15"
    }
}
```

This endpoint will return a specific tag by id

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>`

## Create a New Tag
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Bug",
    "description": "Tickets reporting a bug"
}'
```

> The above command will create a new tag

```json
{
    "message": "Tag has been successfully created",
    "tag": {
        "title": "Bug",
        "description": "Tickets reporting a bug",
        "tag_type": "ticket_tag",
        "created_by": 1,
        "slug": "bug",
        "updated_at": "2026-11-04 10:20:11",
        "created_at": "2026-11-04 10:20:11",
        "id": 2
    }
}
```

This endpoint will create a new tag. The slug is generated from the title.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Ticket tag title
description | string | no | Ticket tag description


## Update a Tag

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "New Customer",
    "description": ""
}'
```

> The above command will update a tag by it's id

```json
{
    "message": "Tag has been updated",
    "tag": {
        "id": 1,
        "tag_type": "ticket_tag",
        "title": "New Customer",
        "slug": "nw-customer",
        "description": "",
        "settings": "",
        "created_by": "1",
        "created_at": "2021-12-01 13:53:20",
        "updated_at": "2021-12-09 12:42:15"
    }
}
```

This endpoint will update a tag by it's id

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Tag title
description | string | no | Tag description


## Delete a Tag
```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete a specific tag by it's id

```json
{
    "message": "Tag has been deleted"
}
```

This end point will delete a specific tag by it's id

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>`
