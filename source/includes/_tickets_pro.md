## Get tickets of a customer

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/customer_tickets/<customer_id>?exclude_ticket_id=138&page=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "tickets": {
        "current_page": 1,
        "data": [
            {
                "id": 125,
                "customer_id": "61",
                "agent_id": "1",
                "mailbox_id": "1",
                "product_id": null,
                "privacy": "private",
                "priority": "normal",
                "client_priority": "normal",
                "status": "closed",
                "title": "Need help with the setup",
                "slug": "need-help-with-the-setup",
                "hash": "b2d9c1a7f1",
                "source": "web",
                "content": "<p>How do I connect my mailbox?</p>",
                "response_count": "3",
                "created_at": "2021-11-30 10:12:44",
                "updated_at": "2021-12-01 09:01:13"
            }
        ],
        "per_page": 15,
        "total": 1,
        "last_page": 1,
        "from": 1,
        "to": 1,
        "next_page_url": null,
        "prev_page_url": null
    }
}
```

This endpoint returns the tickets of a customer (newest first), paginated. It is used to pick tickets to merge. Only tickets the current agent can access are returned.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/customer_tickets/<customer_id>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
customer_id | integer | yes | The customer ID (in the URL).
exclude_ticket_id | integer | no | A ticket ID to leave out of the list, usually the ticket you are merging into.
page | integer | no | Page number. Default 1.
per_page | integer | no | Tickets per page. Default 15.

## Merge tickets

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/merge_tickets?ticket_to_merge[]=125&ticket_to_merge[]=126' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command merges the tickets and returns JSON structured like this:

```json
{
    "message": "Tickets have been merged"
}
```

This endpoint merges one or more tickets into the ticket in the URL. For each merged ticket, its opening message is added to the target ticket as a customer reply, all its replies, attachments, logged time and (for email tickets) CC recipients move to the target ticket, a merge activity note is added, and the merged ticket is deleted. A pending scheduled reply on a merged ticket is not sent on the target ticket. The merge runs in a database transaction: if any ticket fails, nothing is merged.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

The agent needs the `fst_merge_tickets` permission and access to every ticket involved.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/merge_tickets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket_id | integer | yes | The target ticket that the other tickets are merged into (in the URL).
ticket_to_merge[] | array | yes | IDs of the tickets to merge into the target ticket. At most 20 per request, and the target ticket must not be in the list.

## Split a reply into a new ticket

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/split_ticket' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "split_ticket": {
        "conversation_id": 114,
        "customer_id": 61,
        "title": "Question about custom fields",
        "content": "<p>Does Fluent Support have custom fields?</p>",
        "mailbox_id": 1,
        "client_priority": "normal"
    }
}'
```

> The above command splits the reply and returns JSON structured like this:

```json
{
    "message": "Ticket has been split to new ticket",
    "new_ticket_id": 139
}
```

This endpoint creates a new ticket from one reply of a ticket. The reply is removed from the original ticket and its attachments move to the new ticket. An activity note is added to both tickets. A scheduled (unsent) reply cannot be split.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

The agent needs the `fst_split_ticket` permission.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/split_ticket`

### URL Parameters

All fields are sent inside the `split_ticket` object.

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
split_ticket[conversation_id] | integer | yes | ID of the reply (conversation) on this ticket to split out.
split_ticket[customer_id] | integer | yes | Customer of the new ticket.
split_ticket[title] | text | yes | Title of the new ticket.
split_ticket[content] | text | yes | Content of the new ticket (HTML allowed).
split_ticket[mailbox_id] | integer | no | Mailbox of the new ticket. The default mailbox is used when empty.
split_ticket[client_priority] | text | no | Customer priority of the new ticket, e.g. `normal`, `medium`, `critical`.
split_ticket[product_id] | integer | no | Product of the new ticket.
split_ticket[priority] | text | no | Agent priority of the new ticket.

## Sync ticket watchers

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/sync-watchers?watchers[]=2&watchers[]=5' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Watchers have been updated"
}
```

This endpoint replaces the ticket's watchers with the given agents: agents not in the list are removed, new ones are added. Send an empty list to remove all watchers.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/sync-watchers`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
watchers[] | array | no | The agent IDs that should watch the ticket. Each item may be an agent ID or an object with an `id` key.

## Add ticket watchers

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/add_watchers?watchers[]=2&watchers[]=5' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Watchers have been added to this ticket"
}
```

This endpoint adds agents as watchers of a ticket. Existing watchers are kept; agents already watching are ignored.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/add_watchers`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
watchers[] | array | yes | The agent IDs to add as watchers.

## Schedule a reply

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses?content=We have shipped the fix, please update.&conversation_type=response&scheduled_at=1767225600' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command schedules the reply and returns JSON structured like this:

```json
{
    "message": "Reply has been scheduled",
    "response": {
        "id": 115,
        "ticket_id": 138,
        "person_id": 1,
        "conversation_type": "schedule",
        "content": "<p>We have shipped the fix, please update.</p>",
        "source": "web",
        "created_at": "2021-12-03 17:01:21",
        "updated_at": "2021-12-03 17:01:21",
        "attachments": [],
        "person": {
            "id": 1,
            "first_name": "John",
            "last_name": "Doe",
            "full_name": "John Doe",
            "person_type": "agent"
        }
    },
    "ticket": {
        "id": 138,
        "title": "About Features",
        "status": "active"
    },
    "update_data": []
}
```

A reply is scheduled through the regular [reply endpoint](#reply-to-a-ticket-as-agent) by adding `scheduled_at`. The reply is stored with `conversation_type` `schedule` and is sent (with its notifications) at the given time. A ticket can have only one scheduled reply at a time. The pending reply is returned as `scheduled_reply` (with a `schedule` object: `scheduled_at`, `status`, `paused_reason`, `close_ticket`, ...) when you get a ticket.

A scheduled reply is paused instead of sent when the customer replies or the ticket is closed in the meantime; use [resume](#resume-a-scheduled-reply) or [send now](#send-a-scheduled-reply-now) to continue.

<aside class="notice">Scheduling requires Fluent Support Pro. Without it the request fails with "Scheduling replies needs Fluent Support Pro."</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text | yes | The reply content.
conversation_type | text | yes | Must be `response`. Notes cannot be scheduled.
scheduled_at | integer | yes | Unix timestamp to send the reply at. Must be at least a minute from now and at most 90 days ahead.
close_ticket | text | no | `yes` to close the ticket when the reply is sent.
informational_reply | boolean | no | Send as an informational reply when it goes out.
cc_emails[] | array | no | CC email addresses, applied when the reply is sent.
attachments[] | array | no | File hashes of uploaded attachments.

## Update a scheduled reply

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>?content=Updated reply text&scheduled_at=1767312000' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Scheduled reply has been updated"
}
```

This endpoint edits a scheduled reply that has not been sent yet. Only the fields you send are changed. Setting a new `scheduled_at` also resumes a paused reply.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

Only the author of the scheduled reply can change it, unless the agent has the `fst_manage_other_tickets` permission.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
response_id | integer | yes | ID of the scheduled reply (in the URL).
content | text | no | New reply content. Cannot be empty.
scheduled_at | integer | no | New Unix timestamp to send at. At least a minute from now and at most 90 days ahead.
close_ticket | text | no | `yes` or `no`: close the ticket when the reply is sent.
cc_emails[] | array | no | The full CC list for the reply. Send an empty value to clear it.
attachments[] | array | no | The full list of attachment file hashes the reply keeps. Send an empty value to remove all.

## Send a scheduled reply now

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>/send_now' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Response has been added"
}
```

This endpoint sends a scheduled reply right away, ignoring its scheduled time and any pause.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

Only the author can send it, unless the agent has the `fst_manage_other_tickets` permission.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>/send_now`

## Resume a scheduled reply

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>/resume' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Reply will be sent as planned"
}
```

This endpoint resumes a paused scheduled reply ("send as planned"). If its original time is still in the future it is queued again and the message is `Reply will be sent as planned`; if the time has passed, it is sent right away and the message is `Response has been added`.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

Only the author can resume it, unless the agent has the `fst_manage_other_tickets` permission.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>/resume`

## Cancel a scheduled reply

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD'
```

> The above command returns JSON structured like this:

```json
{
    "message": "Scheduled reply has been cancelled",
    "restore": {
        "content": "<p>We have shipped the fix, please update.</p>",
        "cc_emails": [],
        "attachments": [
            {
                "file_hash": "5f1b8e0a1c2d3e4f",
                "title": "screenshot.png",
                "file_size": 48213,
                "file_type": "image/png"
            }
        ]
    }
}
```

This endpoint cancels and deletes a scheduled reply. The `restore` object holds what was written (content, CC emails and released attachments) so it can be put back into the reply editor. Any agent who can reply to the ticket can cancel it; the cancellation is logged in the activity log.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/scheduled_reply/<response_id>`
