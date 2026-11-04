# Saved Replies

<aside class="notice">All saved reply endpoints require Fluent Support Pro. Listing and reading templates is open to any agent with ticket access; creating, updating and deleting needs the manage saved replies (`fst_manage_saved_replies`) or manage settings (`fst_manage_settings`) permission. An agent can update or delete only the templates they created, unless they have `fst_manage_settings`.</aside>

## Get Saved Replies

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the saved reply templates.

```json
{
    "replies": {
        "total": 2,
        "per_page": 10,
        "current_page": 1,
        "last_page": 1,
        "next_page_url": null,
        "prev_page_url": null,
        "from": 1,
        "to": 2,
        "data": [
            {
                "id": 5,
                "title": "Closing Ticket",
                "content": "<p>It has been my pleasure helping you.<br />Closing the tickets here. You can re-open it if you need any assistance.</p>\n<p>Thanks</p>",
                "product_id": null,
                "created_by": 1,
                "created_at": "2021-11-15 18:36:07",
                "updated_at": "2021-11-15 18:36:07",
                "person": {
                    "id": 1,
                    "full_name": "Jane Agent",
                    "photo": "https://secure.gravatar.com/avatar/HASH?s=128"
                },
                "product": null
            },
            {
                "id": 3,
                "title": "Fluent Forms Activation",
                "content": "<p>Hi this is me with your solution</p>",
                "product_id": 4,
                "created_by": 17,
                "created_at": "2021-11-15 11:05:33",
                "updated_at": "2021-11-15 18:36:57",
                "person": null,
                "product": {
                    "id": 4,
                    "title": "Fluent Forms"
                }
            }
        ]
    }
}
```

This endpoint returns all saved reply templates (paginated, newest first).

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/saved-replies`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
page | int | no | Current page, default 1
per_page | int | no | Templates per page, default 10
search | string | no | Search templates
product_id | int | no | Only templates of this product

## Get a Specific Saved Reply

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> This endpoint returns a specific saved reply.

```json
{
    "id": 1,
    "title": "Installation Instruction",
    "content": "<p>This is how you can install</p>\n<p>Thanks</p>",
    "product_id": null,
    "created_by": 1,
    "created_at": "2021-11-11 16:35:54",
    "updated_at": "2021-11-12 18:47:45",
    "person": {
        "id": 1,
        "full_name": "Jane Agent",
        "photo": "https://secure.gravatar.com/avatar/HASH?s=128"
    },
    "product": null
}
```

This endpoint returns a specific saved reply.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>`

## Add New Saved Reply

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Ticket Closing",
    "content": "<p>I am closing this ticket here. Let us know if you have any other issues.</p>"
}'
```

> The above command creates a new saved reply.

```json
{
    "reply": {
        "id": 2,
        "title": "Ticket Closing",
        "content": "<p>I am closing this ticket here. Let us know if you have any other issues.</p>",
        "product_id": null,
        "created_by": 1,
        "created_at": "2021-12-06 18:16:51",
        "updated_at": "2021-12-06 18:16:51",
        "person": {
            "id": 1,
            "full_name": "Jane Agent",
            "photo": "https://secure.gravatar.com/avatar/HASH?s=128"
        },
        "product": null
    },
    "message": "Reply Template has been created"
}
```

This endpoint creates a new saved reply, owned by the current agent.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/saved-replies`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Set a title for the saved reply template
content | string | yes | Set content (HTML) of this reply
product_id | int | no | Set this reply for a specific product, but you can use it anywhere in reply

## Update a Saved Reply Template

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "Update License",
    "content": "<p>Please contact our billing department regarding this query.</p>",
    "product_id": 2
}'
```

> The above command will update a specific saved reply.

```json
{
    "message": "Reply Template has been updated",
    "reply": {
        "id": 1,
        "title": "Update License",
        "content": "<p>Please contact our billing department regarding this query.</p>",
        "product_id": 2,
        "created_by": 1,
        "created_at": "2021-12-01 15:55:28",
        "updated_at": "2021-12-06 18:23:35",
        "person": {
            "id": 1,
            "full_name": "Jane Agent",
            "photo": "https://secure.gravatar.com/avatar/HASH?s=128"
        },
        "product": {
            "id": 2,
            "title": "Fluent Forms"
        }
    }
}
```

This endpoint will update a specific saved reply. All fields are sent again: an omitted `product_id` clears the product.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | string | yes | Update the title for the saved reply template
content | string | yes | Update the content of this reply
product_id | int | no | Update the product

## Delete Saved Reply

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command delete a specific saved reply.

```json
{
    "message": "Selected Reply Template has been deleted"
}
```

This endpoint will delete a specific saved reply.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>`
