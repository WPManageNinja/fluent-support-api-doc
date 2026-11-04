# Customer Portal

The customer portal endpoints act as the currently logged-in WordPress user, who is treated as a support customer (the customer record is found by the user's WordPress user ID only, and created when they open their first ticket if none matches). They are the endpoints the frontend customer portal uses.

When you call them with API credentials, the API user itself becomes the customer. To act for another customer (for example from a remote portal), send an `on_behalf` object with the customer's data (such as `email`, `first_name`, `last_name`, `user_id` or `remote_uid`); the customer is looked up by `remote_uid`, then `user_id`, and by `email` only when no `user_id` is sent and no `remote_uid` match is found. Using `on_behalf` requires a site administrator, or an agent with the `fst_sensitive_data`, `fst_manage_other_tickets` and `fst_manage_settings` permissions; that agent's own mailbox restrictions still apply.

The `<ticket_id>` in these routes is the public ticket number. When public signed ticket links are enabled, the ticket view, reply, close, re-open and file upload endpoints can also be used without login by sending the ticket's `intended_ticket_hash`.

## Get Portal Options
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/public_options' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return the portal options

```json
{
    "support_products": [
        {
            "id": 1,
            "title": "Fluent Forms"
        },
        {
            "id": 2,
            "title": "Fluent Support"
        }
    ],
    "customer_ticket_priorities": {
        "normal": "Normal",
        "medium": "Medium",
        "critical": "Critical"
    }
}
```

This endpoint returns the products and customer priorities that can be chosen when creating a ticket.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/public_options`

## Get Rendered Custom Fields
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/custom-fields-rendered' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return the custom fields for the ticket form

```json
{
    "custom_fields_rendered": {
        "cf_order_number": {
            "type": "text",
            "label": "Order Number",
            "slug": "cf_order_number",
            "required": "no"
        }
    }
}
```

This endpoint returns the public ticket custom fields the current customer can fill in on the ticket form. Custom fields are a Fluent Support Pro feature; without Pro the list is empty.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/custom-fields-rendered`

## Log Out
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/logout' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will log out the current user

```json
{
    "message": "You have been logged out"
}
```

This endpoint logs the current user out of WordPress. It is meant for the portal's browser session.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/logout`

## Get Current User
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/me?with_portal_settings=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return the current user

```json
{
    "user_id": 12,
    "email": "jane@example.com",
    "person": null,
    "permissions": [],
    "portal_settings": {
        "support_products": [
            {
                "id": 1,
                "title": "Fluent Forms"
            }
        ],
        "customer_ticket_priorities": {
            "normal": "Normal",
            "medium": "Medium",
            "critical": "Critical"
        },
        "has_file_upload": true,
        "has_rich_text_editor": true,
        "max_file_size": 2,
        "mime_headings": ["Images", "PDF", "CSV"]
    }
}
```

This endpoint returns the logged-in WordPress user's ID and email. `person` is the agent record and `permissions` the plugin permissions when the user is an agent (otherwise `null` and empty). The same endpoint is also available as `GET /me`.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/me`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
with_portal_settings | text | no | Send any value to also get `portal_settings` (products, priorities and file upload settings)

## Get Current User (Short Route)
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/me' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return the current user

```json
{
    "user_id": 1,
    "email": "john@example.com",
    "person": {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe",
        "email": "john@example.com",
        "person_type": "agent",
        "full_name": "John Doe",
        "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128"
    },
    "permissions": [
        "fst_view_dashboard",
        "fst_manage_own_tickets"
    ]
}
```

Same as `GET customer-portal/me`, outside the `customer-portal` prefix. It accepts the same `with_portal_settings` parameter.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/me`

## Get Countries
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/options/countries' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return the country list

```json
{
    "countries": {
        "AF": "Afghanistan",
        "AX": "Aland Islands",
        "AL": "Albania",
        "DZ": "Algeria"
    }
}
```

This endpoint returns the country list (code => name) used by the admin and portal forms. Agents, logged-in customers and signed ticket links can call it.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/options/countries`

## Get Customer Tickets
```shell
curl --location -g --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets?filter_type=open&per_page=10&page=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return the customer's tickets

```json
{
    "tickets": {
        "total": 1,
        "per_page": 10,
        "current_page": 1,
        "last_page": 1,
        "next_page_url": null,
        "prev_page_url": null,
        "from": 1,
        "to": 1,
        "data": [
            {
                "id": 143,
                "serial_number": "12",
                "title": "Can I use Fluent Support with Fluent Forms",
                "content": "<p>Hello, ...</p>",
                "status": "active",
                "client_priority": "normal",
                "product_id": "1",
                "mailbox_id": "1",
                "customer_id": "83",
                "agent_id": "1",
                "response_count": "2",
                "created_at": "2021-12-04 16:06:33",
                "updated_at": "2021-12-05 10:00:12",
                "human_date": "3 days ago",
                "preview_response": {
                    "id": 512,
                    "ticket_id": "143",
                    "person_id": "1",
                    "conversation_type": "response",
                    "content": "<p>Yes, you can.</p>",
                    "created_at": "2021-12-05 10:00:12"
                }
            }
        ]
    }
}
```

This endpoint returns the current customer's tickets with a preview of the latest response. If the user has no customer record yet, an empty page is returned. A blocked customer gets a 403 error.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Records per page (1 to 100), default 10
page | int | no | Page Number for Pagination, default 1
filter_type | text | no | `open` (new, active, on-hold) or `closed`; any other value returns all
search | text | no | Search tickets
filters[product_id] | int | no | Filter by product ID
sorting[sort_by] | text | no | Column to sort by, default `created_at`
sorting[sort_type] | text | no | `asc` or `desc` (default)
on_behalf | object | no | Customer data to act for (see the top of this section)

## Create a Ticket as Customer
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "title": "I need help with my order",
    "content": "<p>My order has not arrived yet.</p>",
    "product_id": 1,
    "client_priority": "normal",
    "attachments": ["8c7d0a5e4b0c6e2d3f1a"]
}'
```

> The above command will create a new ticket for the current customer

```json
{
    "message": "Ticket has been created successfully",
    "ticket": {
        "id": 150,
        "serial_number": 13,
        "title": "I need help with my order",
        "content": "<p>My order has not arrived yet.</p>",
        "status": "new",
        "client_priority": "normal",
        "product_id": 1,
        "product_source": "local",
        "mailbox_id": 1,
        "customer_id": 83,
        "source": "web",
        "created_at": "2021-12-06 13:27:23",
        "updated_at": "2021-12-06 13:27:23"
    }
}
```

This endpoint creates a ticket for the current customer (the customer record is created if it does not exist). Required fields can be extended by the ticket form settings (for example a required product) and by Pro custom fields. If reCAPTCHA is enabled for the ticket form, a valid `g-recaptcha-response` is required (not for `on_behalf` requests).

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | text | yes | Ticket subject
content | html | yes | Ticket details
product_id | int | no | Product ID (required when the ticket form requires a product)
client_priority | text | no | Customer priority, e.g. `normal`, `medium`, `critical`
mailbox_id | int | no | Mailbox (business box) ID; the default mailbox is used when omitted
attachments[] | array | no | Attachment hashes returned by the file upload endpoint
custom_data | object | no | Custom field values keyed by field slug (Pro)
g-recaptcha-response | text | no | reCAPTCHA token, when reCAPTCHA is enabled
on_behalf | object | no | Customer data to act for (see the top of this section)

## Get a Customer Ticket
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a ticket with its responses

```json
{
    "ticket": {
        "id": 143,
        "serial_number": "12",
        "title": "Can I use Fluent Support with Fluent Forms",
        "content": "<p>Hello, ...</p>",
        "status": "active",
        "client_priority": "normal",
        "product_id": "1",
        "customer_id": "83",
        "agent_id": "1",
        "created_at": "2021-12-04 16:06:33",
        "updated_at": "2021-12-05 10:00:12",
        "human_date": "3 days ago",
        "custom_fields": {},
        "customer": {
            "id": 83,
            "first_name": "John",
            "last_name": "Doe",
            "full_name": "John Doe",
            "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128",
            "avatar": null,
            "person_type": "customer"
        },
        "agent": {
            "id": 1,
            "first_name": "Jane",
            "last_name": "Doe",
            "full_name": "Jane Doe",
            "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
            "avatar": null,
            "person_type": "agent",
            "title": "Support Engineer"
        },
        "product": {
            "id": 1,
            "title": "Fluent Forms"
        },
        "attachments": []
    },
    "responses": [
        {
            "id": 512,
            "serial": "1",
            "ticket_id": "143",
            "person_id": "1",
            "conversation_type": "response",
            "content": "<p>Yes, you can.</p>",
            "source": "web",
            "is_important": "no",
            "created_at": "2021-12-05 10:00:12",
            "updated_at": "2021-12-05 10:00:12",
            "human_date": "2 days ago",
            "agent_feedback": "like",
            "person": {
                "id": 1,
                "first_name": "Jane",
                "last_name": "Doe",
                "full_name": "Jane Doe",
                "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
                "avatar": null,
                "person_type": "agent",
                "title": "Support Engineer"
            },
            "attachments": []
        }
    ],
    "sign_on_id": "83"
}
```

This endpoint returns one of the current customer's tickets with its responses (newest first). A ticket the customer cannot access returns a 404 error. `custom_fields` (Pro) holds the public custom field values, `closed_by_person` is included for closed tickets, and `created_by_agent` is included when an agent opened the ticket for the customer. `agent_feedback` appears on rated responses when agent feedback is enabled (Pro).

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
intended_ticket_hash | text | no | The ticket hash from a signed ticket link
on_behalf | object | no | Customer data to act for (see the top of this section)

## Reply to a Ticket as Customer
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/responses' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "content": "<p>Thanks, that worked.</p>",
    "close_ticket": "no"
}'
```

> The above command will add a customer reply to a ticket

```json
{
    "message": "Reply has been added",
    "response": {
        "id": 520,
        "ticket_id": 143,
        "person_id": 83,
        "conversation_type": "response",
        "content": "<p>Thanks, that worked.</p>",
        "source": "web",
        "created_at": "2021-12-06 13:27:23",
        "updated_at": "2021-12-06 13:27:23",
        "person": {
            "id": 83,
            "first_name": "John",
            "last_name": "Doe",
            "full_name": "John Doe",
            "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128",
            "avatar": null,
            "person_type": "customer"
        },
        "attachments": []
    },
    "ticket": {
        "id": 143,
        "serial_number": 12,
        "status": "active",
        "resolved_at": null
    }
}
```

This endpoint adds a customer reply to a ticket. Replying to a closed ticket re-opens it, unless `close_ticket` is `yes`.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/responses`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | html | yes | Reply content
close_ticket | text | no | `yes` to close the ticket with this reply
attachments[] | array | no | Attachment hashes returned by the file upload endpoint
intended_ticket_hash | text | no | The ticket hash from a signed ticket link
on_behalf | object | no | Customer data to act for (see the top of this section)

## Close a Ticket as Customer
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/close' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will close a ticket

```json
{
    "message": "Ticket has been closed",
    "ticket": {
        "id": 143,
        "serial_number": 12,
        "status": "closed",
        "resolved_at": "2021-12-06 13:27:23"
    }
}
```

This endpoint closes one of the current customer's tickets.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/close`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
intended_ticket_hash | text | no | The ticket hash from a signed ticket link
on_behalf | object | no | Customer data to act for (see the top of this section)

## Re-open a Ticket as Customer
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/re-open' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will re-open a closed ticket

```json
{
    "message": "Ticket has been opened again",
    "ticket": {
        "id": 143,
        "serial_number": 12,
        "status": "active",
        "resolved_at": null
    }
}
```

This endpoint re-opens one of the current customer's closed tickets.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/re-open`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
intended_ticket_hash | text | no | The ticket hash from a signed ticket link
on_behalf | object | no | Customer data to act for (see the top of this section)

## Rate an Agent Response
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/agent-feedback' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "conversation_id": 512,
    "approval_status": "like"
}'
```

> The above command will rate an agent's response

```json
{
    "id": 77,
    "object_type": "conversation_meta",
    "object_id": 512,
    "key": "agent_feedback_ratings",
    "value": "like",
    "created_at": "2021-12-06 13:27:23",
    "updated_at": "2021-12-06 13:27:23"
}
```

This endpoint lets the customer like or dislike an agent's response on their ticket. Sending the same rating again removes it. The rating record is returned (also when it was just removed).

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets/<ticket_id>/agent-feedback`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
conversation_id | int | yes | ID of the agent response on this ticket
approval_status | text | yes | `like` or `dislike`
on_behalf | object | no | Customer data to act for (see the top of this section)

## Upload Ticket Files
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/ticket_file_upload' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'file=@"/path/to/screenshot.png"' \
--form 'ticket_id="12"'
```

> The above command will upload a file for a ticket

```json
{
    "attachments": [
        "8c7d0a5e4b0c6e2d3f1a9b7c6d5e4f3a"
    ]
}
```

This endpoint uploads a file as a temporary attachment and returns its hash. Send the returned hashes as `attachments` when creating a ticket or a reply. File size and type limits come from the global business settings; customers cannot upload when the file upload field is disabled. With `type=direct_paste` the response's `attachments` is the file's URL instead of a list of hashes.

The same endpoint is also available as `POST /ticket_file_upload`, which agents use from the admin editor (agents need a ticket management permission or `fst_draft_reply`, `fst_manage_settings` or `fst_manage_saved_replies`).

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/ticket_file_upload`

`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket_file_upload`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
file | file | yes | The file to upload (multipart form field `file`; no other file fields are accepted)
ticket_id | int | no | The ticket the file belongs to (public ticket number for customers; omit when creating a new ticket)
type | text | no | `direct_paste` for an image pasted into the editor
intended_ticket_hash | text | no | The ticket hash from a signed ticket link
on_behalf | object | no | Customer data to act for (see the top of this section)

## Search Docs
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/search-doc?search=refund' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will search the knowledge base docs

```json
[
    {
        "title": "How to request a refund",
        "link": "https://yourdomain.com/docs/how-to-request-a-refund/"
    }
]
```

This endpoint searches the documentation posts configured in the ticket form settings and returns suggested docs while a customer writes a ticket. It returns an empty list when doc suggestions are disabled or no search is given. This endpoint is public.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/search-doc`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
search | text | yes | Search keyword
