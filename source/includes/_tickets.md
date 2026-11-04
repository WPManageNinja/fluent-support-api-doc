# Tickets

## Get All Tickets

```shell
curl "https://yourdomain.com/wp-json/fluent-support/v2/tickets" \
  -H "Authorization: BASIC API_USERNAME:API_PASSWORD"
```
> The above command returns JSON structured like this:

```json
{
    "tickets": {
        "total": 114,
        "per_page": 15,
        "current_page": 1,
        "last_page": 8,
        "next_page_url": "/wp-json/fluent-support/v2/tickets?page=2",
        "prev_page_url": null,
        "from": 1,
        "to": 15,
        "data": [
            {
                "id": 1,
                "customer_id": "2",
                "agent_id": "1",
                "mailbox_id": "1",
                "product_id": "2",
                "product_source": "local",
                "privacy": "private",
                "priority": "normal",
                "client_priority": "normal",
                "status": "active",
                "title": "Is Fluent Support comes with Slack integration?",
                "slug": "is-fluent-support-comes-with-slack-integration",
                "hash": "a51ad12525",
                "content_hash": "d3e2093a3286a81b7a40931b82687eb8",
                "message_id": null,
                "source": "web",
                "content": "<p>Hello, Is fluent support has slack integration?</p>",
                "secret_content": null,
                "last_agent_response": "2021-11-24 16:45:25",
                "last_customer_response": "2021-11-18 15:26:18",
                "waiting_since": "2021-11-24 16:45:25",
                "response_count": "11",
                "first_response_time": "518766",
                "total_close_time": null,
                "resolved_at": null,
                "closed_by": null,
                "created_at": "2021-11-18 15:26:18",
                "updated_at": "2021-11-24 16:45:25",
                "live_activity": [],
                "customer": {
                    "first_name": "Hillary",
                    "last_name": "Sander",
                    "email": "hillary@example.com",
                    "id": 2,
                    "full_name": "Hillary Sander",
                    "photo": "https://www.gravatar.com/avatar/905f39c5c1129f13a3850723046d0233?s=128"
                },
                "agent": {
                    "first_name": "John",
                    "last_name": "Doe",
                    "id": 1,
                    "full_name": "John Doe",
                    "photo": "https://www.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=128"
                },
                "product": null,
                "tags": [],
                "preview_response": {
                    "id": 47,
                    "serial": "1",
                    "ticket_id": "1",
                    "person_id": "1",
                    "conversation_type": "response",
                    "content": "<p>Yes, but for this you have to install the pro version.</p>",
                    "source": "web",
                    "content_hash": "2c7a108b6989f42c3fdeddd4fad78c51",
                    "message_id": null,
                    "is_important": "no",
                    "created_at": "2021-11-24 16:45:25",
                    "updated_at": "2021-11-24 16:45:25"
                }
            }
        ]
    }
}
```

This endpoint retrieves all tickets.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets`

### URL Parameters

Parameter | Type | Description | Default
--------- | ------- | ----------- | -------
| per_page |  int | Records per page | 15 |
| page |    int   | Page Number for Pagination | 1 |
| search | string | Search by ticket title, content or ticket number. You can also search a column with `column:value` | |
| customer_id | int | Only return tickets of this customer | |
| order_by | string | Sort column. One of `id, created_at, updated_at, title, status, priority, client_priority, product_id, waiting_since, last_agent_response, last_customer_response, response_count`. Any other value falls back to `id` | id |
| order_type | string | `ASC` or `DESC` | DESC |
| filters | array | Simple filters (see below). Ignored when `filter_type` is `advanced` | |
| filter_type | string | `simple` or `advanced`. Use `advanced` with `advanced_filters` (advanced filters require Fluent Support Pro) | simple |
| advanced_filters | string/array | JSON encoded filter groups: `[[{"source": ["tickets", "status"], "operator": "in", "value": ["new"]}]]`. Groups are joined with OR, rows inside a group with AND | |

***Possible Filter Values (`filters[key]`):***

- status_type (open/active/new/closed/all)
- product_id (int or array of int)
- agent_id (int, array of int, or `unassigned`)
- watcher (`watcher`: together with agent_id returns tickets those agents watch, Pro only)
- agent_group (int or array of agent group ids)
- priority (Agent Ticket Priority, value or array)
- client_priority (Customer Ticket Priority, value or array)
- waiting_for_reply (yes)
- mailbox_id (int or array of int)
- ticket_tags (array of tag ids)
- deep_search (1: also search the customer's name/email and the replies)
- created_after / created_before (Y-m-d H:i:s)

<aside class="success">
Remember — Use authentication Headers
</aside>

## Get a Specific Ticket


```shell
curl "https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ID>" \
  -H "Authorization: BASIC API_USERNAME:API_PASSWORD"
```

> The above command returns JSON structured like this:

```json
{
    "ticket": {
        "id": 133,
        "customer_id": "72",
        "agent_id": null,
        "mailbox_id": "1",
        "product_id": "1",
        "ticket_type_id": null,
        "product_source": null,
        "privacy": "private",
        "priority": "normal",
        "client_priority": "normal",
        "status": "new",
        "title": "Does Fluent Support comes with REST API?",
        "slug": "does-fluent-support-comes-with-rest-api",
        "hash": "2261a20227",
        "content_hash": "f1faca333a4d6acfdadc49febec3dcf3",
        "message_id": null,
        "source": null,
        "content": "<p>Is REST API available in Fluent Support?</p>\n",
        "secret_content": null,
        "last_agent_response": null,
        "last_customer_response": "2021-12-02 20:15:56",
        "waiting_since": "2021-12-02 20:15:56",
        "response_count": "0",
        "first_response_time": null,
        "total_close_time": null,
        "resolved_at": null,
        "closed_by": null,
        "created_at": "2021-12-02 20:15:56",
        "updated_at": "2021-12-02 20:15:56",
        "live_activity": [
            {
                "email": "john@example.com",
                "first_name": "John",
                "last_name": "Doe",
                "full_name": "John Doe",
                "photo": "https://www.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=128"
            }
        ],
        "custom_fields": [],
        "customer": {
            "id": 72,
            "first_name": "Aston",
            "last_name": "Ager",
            "email": "aston@example.com",
            "title": null,
            "avatar": null,
            "person_type": "customer",
            "status": "active",
            "ip_address": null,
            "last_ip_address": null,
            "address_line_1": null,
            "address_line_2": null,
            "city": null,
            "zip": null,
            "state": null,
            "country": null,
            "note": null,
            "hash": "d755687879ea116e23bfa09817d40ab8",
            "user_id": null,
            "description": null,
            "remote_uid": null,
            "last_response_at": "2021-12-02 20:15:56",
            "created_at": "2021-12-02 13:58:02",
            "updated_at": "2021-12-02 20:15:56",
            "profile_edit_url": "",
            "full_name": "Aston Ager",
            "photo": "https://www.gravatar.com/avatar/4985ab7f38077814d5b139329c37786f?s=128"
        },
        "agent": null,
        "product": {
            "id": 1,
            "source_uid": null,
            "mailbox_id": null,
            "title": "Fluent Support",
            "description": "",
            "settings": null,
            "source": "local",
            "created_by": null,
            "created_at": "2021-11-29 17:56:07",
            "updated_at": "2021-11-29 17:56:07"
        },
        "mailbox": {
            "id": 1,
            "name": "Fluent Support",
            "slug": "fluent-support",
            "box_type": "web",
            "email": "support@example.com",
            "mapped_email": "",
            "email_footer": "<p>Hi There,</p>\n<p>Here is your data.</p>\n<p>Customer First name: {{customer.first_name}}</p>\n<p>Customer Last name: {{customer.last_name}}</p>\n<p>Customer Email: {{customer.email}}</p>\n<p>Ticket ID: {{ticket.id}}</p>\n<p>Ticket Public URL: {{ticket.public_url}}</p>\n<p>Ticket Title: {{ticket.title}}</p>",
            "settings": {
                "admin_email_address": "support@example.com"
            },
            "avatar": "",
            "created_by": "1",
            "is_default": "yes",
            "created_at": "2021-11-18 15:08:42",
            "updated_at": "2021-11-25 18:45:27"
        },
        "tags": [],
        "attachments": []
    },
    "responses": [],
    "agent_id": 1
}
```

Endpoint to retrieve a specific ticket by Ticket ID.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ----------- | -----
with[] | array | Limit the loaded ticket relations. Allowed values: `customer, agent, product, mailbox, tags, attachments`. All of them are loaded by default
with_data[] | array | Get additional ticket data

***Possible with_data parameters:***

- fluentcrm_profile (requires FluentCRM)

The customer's other tickets and extra profile widgets are returned by the [Get Ticket Widgets](#get-ticket-widgets) endpoint.

## Create a new Ticket using agent

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets?ticket[customer_id]=72&ticket[mailbox_id]=1&ticket[title]=Does Fluent Support comes with REST API?&ticket[content]=Is REST API available in Fluent Support?&ticket[product_id]=1&ticket[client_priority]=normal&ticket[create_customer]=no&ticket[create_wp_user]=no' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command creates a new ticket in Fluent support and returns the data in JSON.

```json
{
    "message": "Ticket has been created successfully",
    "ticket": {
        "customer_id": "72",
        "mailbox_id": "1",
        "title": "Does Fluent Support comes with REST API?",
        "content": "Is REST API available in Fluent Support?",
        "product_id": "1",
        "client_priority": "normal",
        "slug": "does-fluent-support-comes-with-rest-api",
        "hash": "2261a20227",
        "last_customer_response": "2021-12-02 20:15:56",
        "content_hash": "f1faca333a4d6acfdadc49febec3dcf3",
        "created_at": "2021-12-02 20:15:56",
        "updated_at": "2021-12-02 20:15:56",
        "waiting_since": "2021-12-02 20:15:56",
        "id": 133,
        "custom_fields": [],
        "mailbox": {
            "id": 1,
            "name": "Fluent Support",
            "slug": "fluent-support",
            "box_type": "web",
            "email": "support@example.com",
            "mapped_email": "",
            "email_footer": "<p>Hi There,</p>\n<p>Here is your data.</p>\n<p>Customer First name: {{customer.first_name}}</p>\n<p>Customer Last name: {{customer.last_name}}</p>\n<p>Customer Email: {{customer.email}}</p>\n<p>Ticket ID: {{ticket.id}}</p>\n<p>Ticket Public URL: {{ticket.public_url}}</p>\n<p>Ticket Title: {{ticket.title}}</p>",
            "settings": {
                "admin_email_address": "support@example.com"
            },
            "avatar": "",
            "created_by": "1",
            "is_default": "yes",
            "created_at": "2021-11-18 15:08:42",
            "updated_at": "2021-11-25 18:45:27"
        },
        "product": {
            "id": 1,
            "source_uid": null,
            "mailbox_id": null,
            "title": "Fluent Support",
            "description": "",
            "settings": null,
            "source": "local",
            "created_by": null,
            "created_at": "2021-11-29 17:56:07",
            "updated_at": "2021-11-29 17:56:07"
        },
        "agent": null
    }
}
```

This endpoint creates a new ticket. Using this endpoint you will be able to create a new Customer as well as a WP user. When `ticket[customer_id]` is empty, the customer is looked up by `newCustomer[email]`: an existing customer with that email is reused as it is, otherwise a new customer is created from `newCustomer[first_name]`, `newCustomer[last_name]` and `newCustomer[email]`.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets`

### URL Parameters to create a ticket

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
ticket[customer_id] | int | no | The customer for whom you are creating this ticket. Required unless `newCustomer[email]` is sent.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[mailbox_id] | int | no | The mailbox. The default mailbox is used when empty.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.
ticket[priority] | text | no | The agent ticket priority (`normal`, `medium`, `critical`).
ticket[agent_id] | int | no | Assign the ticket to this agent. Ignored unless the current user has the assign agents permission (`fst_assign_agents`).
ticket[agent_initiated] | text(yes/no) | no | Use `yes` to start the conversation as the agent: `ticket[content]` is added as the agent's first response and the ticket's opening message becomes "{agent} started this conversation".
ticket[custom_fields] | array | no | Custom field values keyed by field slug (requires Fluent Support Pro).
ticket[create_customer] | text(yes/no) | no | When `yes`, `newCustomer[email]` is validated as required. It does not decide whether a customer is created; that happens whenever `ticket[customer_id]` is empty.
ticket[create_wp_user] | text(yes/no) | no | Default value should be no or blank
attachments[] | array | no | File hashes of uploaded attachments.


###  URL Parameters to Create customer during ticket creation:

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket[create_customer] | text(yes/no) | no | Pass yes as value to require `newCustomer[email]`
newCustomer[email] | email | yes | Add your customer email. An existing customer with this email is reused.
newCustomer[first_name] | text | no | Add your customer first name.
newCustomer[last_name] | text | no | Add your customer last name.
ticket[create_wp_user] | text(yes/no) | no | Default value should be no or blank
ticket[mailbox_id] | int | no | The mailbox. The default mailbox is used when empty.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.

###  URL Parameters to Create customer and WP user during ticket creation:

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket[create_customer] | text(yes/no) | no | Pass yes as value to require `newCustomer[email]`
newCustomer[email] | email | yes | Add your customer email.
newCustomer[first_name] | text | no | Add your customer first name.
newCustomer[last_name] | text | no | Add your customer last name.
ticket[create_wp_user] | text(yes/no) | yes | Pass yes as value to create WP user
newCustomer[username] | text | yes | WP username. If the username already exists, no WP user is created and the ticket is still created.
newCustomer[password] | password | no | WP user's password. A random password is generated when empty.
ticket[mailbox_id] | int | no | The mailbox. The default mailbox is used when empty.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.


## Create a new Ticket using customer

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets?title=About Features&content=Does Fluent Support has custom fields' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command creates a new ticket in Fluent Support and returns the data in JSON.

```json
    {
    "message": "Ticket has been created successfully",
    "ticket": {
        "title": "About Features",
        "content": "Does Fluent Support has custom fields?",
        "customer_id": 27,
        "product_source": "local",
        "mailbox_id": 1,
        "client_priority": "",
        "source": "web",
        "slug": "about-features",
        "last_customer_response": "2021-12-03 15:31:00",
        "created_at": "2021-12-03 15:31:00",
        "updated_at": "2021-12-03 15:31:00",
        "waiting_since": "2021-12-03 15:31:00",
        "id": 138,
        "custom_fields": [],
        "product": null,
        "agent": null
    }
}
```

This endpoint creates a new ticket.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | text | yes | Specify the ticket title.
content | text | yes | Specify the ticket content.
client_priority | text | no | Specify the ticket priority.
product_id | int | no | The product of the ticket. Required when the ticket form setting "product required" is on and products exist.
mailbox_id | int | no | The mailbox. The default mailbox is used when empty or not found.
attachments[] | array | no | File hashes of files uploaded through `customer-portal/ticket_file_upload`.
custom_data | array | no | Use custom field slug to send value to the field custom field.
g-recaptcha-response | text | no | The reCAPTCHA token. Required when reCAPTCHA is enabled for the ticket form.

## Reply to a ticket as agent

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses?content=Yes, fluent support does have custom fields.&conversation_type=response' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command add reply to a ticket and returns the data in JSON.

```json
    {
    "message": "Response has been added",
    "response": {
        "person_id": 1,
        "ticket_id": 138,
        "conversation_type": "response",
        "content": "<p>Yes, fluent support does have custom fields.</p>\n",
        "source": "web",
        "content_hash": "c7d7f144acde427c2ff8c7471cd3b715",
        "updated_at": "2021-12-03 17:01:21",
        "created_at": "2021-12-03 17:01:21",
        "id": 114,
        "person": {
            "id": 1,
            "first_name": "John",
            "last_name": "Doe",
            "email": "john@example.com",
            "title": "Developer",
            "avatar": null,
            "person_type": "agent",
            "status": "active",
            "ip_address": null,
            "last_ip_address": null,
            "address_line_1": null,
            "address_line_2": null,
            "city": null,
            "zip": null,
            "state": null,
            "country": null,
            "note": null,
            "hash": "890d34ca8a81de55cef0c5b506887604",
            "user_id": "1",
            "description": null,
            "remote_uid": null,
            "last_response_at": null,
            "created_at": "2021-11-18 15:07:45",
            "updated_at": "2021-12-02 20:14:35",
            "full_name": "John Doe",
            "photo": "https://www.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=128"
        }
    },
    "ticket": {
        "id": 138,
        "customer_id": "61",
        "agent_id": "1",
        "mailbox_id": "1",
        "product_id": null,
        "ticket_type_id": null,
        "product_source": "local",
        "privacy": "private",
        "priority": "",
        "client_priority": "",
        "status": "active",
        "title": "About Features",
        "slug": "about-features",
        "hash": "c1e880131",
        "content_hash": "1e4ef873fb4010ca6d1e5da68b5a45a8",
        "message_id": null,
        "source": "web",
        "content": "Does Fluent Support has custom fields?",
        "secret_content": null,
        "last_agent_response": "2021-12-03 16:57:21",
        "last_customer_response": "2021-12-03 15:31:00",
        "waiting_since": "2021-12-03 16:57:21",
        "response_count": 2,
        "first_response_time": "5181",
        "total_close_time": null,
        "resolved_at": null,
        "closed_by": null,
        "created_at": "2021-12-03 15:31:00",
        "updated_at": "2021-12-03 17:01:21"
    },
    "update_data": []
}
```

This endpoint add reply to a ticket.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text | yes | Specify the ticket content.
conversation_type | text | no | `response` (default) to reply to the customer, `note` to add an internal note, or `draft_response` to save a reply for approval (for agents with draft-only permission).
close_ticket | text | no | To close the ticket use yes otherwise the value will be no.
attachments[] | array | no | File hashes of attachments uploaded by the current agent.
cc_emails[] | array | no | CC email addresses for the reply.
informational_reply | boolean | no | Send the reply as an informational reply.
scheduled_at | int | no | Unix timestamp to send the reply at instead of now. Requires Fluent Support Pro, see [Schedule a reply](#schedule-a-reply).
last_conversation_id | int | no | ID of the latest conversation the agent has seen. If someone else replied after it, the reply is not saved and the request fails with HTTP 409, code `fs_newer_responses`, message `Someone replied to this ticket while you were writing.` and the newer replies in `new_responses`.
force_send | text(yes/no) | no | Use `yes` to skip the `last_conversation_id` check.

## Update any reply

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses/<response_id>?content=Let me know the update.' \

```

> The above command will update a specific ticket.

```json
    {
        "message": "Selected response has been updated",
        "response": {
            "id": 106,
            "serial": "1",
            "ticket_id": "106",
            "person_id": "1",
            "conversation_type": "response",
            "content": "Let me know the update.",
            "source": "web",
            "content_hash": "f681747a752c36a8e64c755a635aa76f",
            "message_id": null,
            "is_important": "no",
            "created_at": "2021-12-01 11:50:57",
            "updated_at": "2021-12-04 15:07:39"
        }
    }
```

This endpoint will update a specific reply.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses/<response_id>?content=Let me know the update.`

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text | yes | Specify the ticket content.


## Delete ticket(s)

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-actions?ticket_ids[]=101&ticket_ids[]=102&bulk_action=delete_tickets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command delete ticket(s) and returns the data in JSON.

```json
    {
        "message": "1 tickets have been deleted"
    }
```

This endpoint will delete ticket(s). The agent needs the delete tickets permission (`fst_delete_tickets`). Tickets the agent can not access are skipped.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-actions`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket_ids[] | array | yes | IDs of the tickets to delete, one `ticket_ids[]` per ID (for example `ticket_ids[]=101&ticket_ids[]=102`). At most 200 per request.
bulk_action | text | yes | To delete ticket(s) use  ``delete_tickets`` as ``bulk_action``

## Delete reply



```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses/<response_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command delete a specific reply of a ticket and returns the data in JSON.

```json
    {
        "message": "Selected response has been deleted"
    }
```

This endpoint will delete ticket reply.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses/<response_id>`

## Add tag to ticket


```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/tags?tag_id=<tag_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command add tag to ticket and return the data in JSON.

```json
    {
        "message": "Tag has been added to this ticket",
        "tags": []
    }
```

This endpoint will add tag to a ticket.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/tags?tag_id=<tag_id>`

## Remove tag from a ticket

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/tags/<tag_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command will remove tag from a ticket and return the data in JSON.

```json
    {
        "message": "Tag has been removed from this ticket",
        "tags": []
    }
```

This endpoint will remove tag from a ticket.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/tags/<tag_id>`

## Update ticket property
***This endpoint update ticket properties like agent, product etc. It takes two parameters prop_name & prop_value, prop_name defines the property key and prop_value defines value***


```shell

curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/property?prop_name=client_priority&prop_value=medium' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command update properties associate with the ticket.

```json
    {
        "message": "Client priority has been updated",
        "update_data": []
    }
```

This endpoint will update properties associate with the ticket.

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/property`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
prop_name | text | yes | prop_name's are ```title, status, client_priority, priority, agent_id, product_id```. A ticket can not be closed or reopened with this endpoint, use the close and re-open endpoints. `agent_id` needs the assign agents permission.
prop_value | text/int | yes | This one will take value base on the prop_name if the prop_name takes id then you have to use a integer number otherwise you will use string.

## Close a ticket

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/close?close_ticket_silently=no' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command closes the ticket and returns the data in JSON.

```json
    {
        "message": "Ticket has been closed",
        "ticket": {
            "id": 138,
            "customer_id": "61",
            "agent_id": "1",
            "mailbox_id": "1",
            "status": "closed",
            "title": "About Features",
            "resolved_at": "2021-12-05 10:12:45",
            "closed_by": 1,
            "total_close_time": 153345,
            "created_at": "2021-12-03 15:31:00",
            "updated_at": "2021-12-05 10:12:45"
        }
    }
```

This endpoint will close a ticket. Closing an already closed ticket returns the ticket unchanged.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/close`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
close_ticket_silently | text(yes/no) | no | Use `yes` to close the ticket silently: the ticket closed hooks do not fire, so no close notification email or close automations run. Default `no`.

## Re-open a ticket

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/re-open' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command re-opens a closed ticket and returns the data in JSON.

```json
    {
        "message": "Ticket has been opened again",
        "ticket": {
            "id": 138,
            "customer_id": "61",
            "agent_id": "1",
            "mailbox_id": "1",
            "status": "active",
            "title": "About Features",
            "resolved_at": null,
            "created_at": "2021-12-03 15:31:00",
            "updated_at": "2021-12-06 09:00:12"
        }
    }
```

This endpoint will re-open a closed ticket. The ticket status becomes `active`.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/re-open`

## Delete a ticket

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/delete' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command deletes a single ticket and returns the data in JSON.

```json
    {
        "message": "Ticket has been deleted successfully"
    }
```

This endpoint will delete a single ticket with its responses and attachments. The agent needs the delete tickets permission.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/delete`

## Update ticket content

```shell

curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/content?content=<p>Updated opening message</p>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command replaces the opening message of the ticket and returns the data in JSON.

```json
    {
        "message": "The opening message has been updated",
        "response": {
            "content": "<p>Updated opening message</p>"
        }
    }
```

This endpoint replaces the ticket's opening message (the content the customer submitted), for example to remove sensitive data. Only site administrators (`manage_options`) can use it.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/content`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text(HTML) | yes | The new opening message. It can not be empty.

## Change ticket customer

```shell

curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/change-customer?customer=<customer_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command moves the ticket to another customer and returns the data in JSON.

```json
    {
        "message": "Customer has been updated"
    }
```

This endpoint changes the customer of a ticket. The agent needs the sensitive data permission (`fst_sensitive_data`).

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/change-customer`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
customer | int | yes | ID of the new customer. It must be different from the current customer.

## Get ticket custom data

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/custom-data' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "custom_data": {
            "cf_order_number": "1024",
            "cf_website": "https://example.com"
        },
        "rendered_fields": {
            "cf_order_number": {
                "type": "text",
                "label": "Order Number",
                "slug": "cf_order_number"
            }
        }
    }
```

This endpoint returns the custom field values of a ticket and the field definitions for the admin view. Without Fluent Support Pro both values are empty arrays.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/custom-data`

## Get ticket widgets

```shell

curl --location -g --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/widgets?page=1&with[]=extra_widgets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "other_tickets": [
            {
                "id": 120,
                "title": "Can not activate the license",
                "status": "closed",
                "ticket_number": null,
                "serial_number": null,
                "created_at": "2021-11-20 10:15:00",
                "updated_at": "2021-11-21 08:00:00"
            }
        ],
        "other_tickets_total": 6,
        "other_tickets_more": true,
        "extra_widgets": {
            "fluentcrm": {
                "title": "FluentCRM Profile",
                "body_html": "..."
            }
        }
    }
```

This endpoint returns the customer's other tickets (5 per page) and, when requested, the extra profile widgets provided by integrations (WooCommerce, FluentCRM, EDD etc.).

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/widgets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
page | int | no | Page of the other tickets list. Default 1.
with[] | array | no | Use `extra_widgets` to include the integration profile widgets.

## Get mentionable agents

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/mentionable-agents?search=raf&limit=20' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "agents": [
            {
                "id": "1",
                "first_name": "John",
                "last_name": "Doe",
                "email": "john@example.com"
            }
        ]
    }
```

This endpoint returns the agents who can access the ticket and can therefore be @mentioned in a note.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/mentionable-agents`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
search | text | no | Search agents by name or email.
limit | int | no | Maximum number of agents (1 to 50). Default 20.

## Save a reply draft

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/draft?content=<p>Work in progress reply</p>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command saves the current agent's reply draft and returns the data in JSON.

```json
    {
        "message": "Draft has been added",
        "draftID": 245
    }
```

This endpoint saves (auto-saves) the current agent's unsent reply for a ticket. Each agent has one draft per ticket; if a draft already exists it is updated and the message is `Draft has been updated`. The whole request payload is stored as the draft value.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/draft`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text(HTML) | yes | The draft reply content.
conversation_type | text | no | Any other reply fields (for example `conversation_type`, `close_ticket`) are stored with the draft as they are.

## Get the reply draft

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/draft' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "draft": {
            "id": 245,
            "object_type": "_fs_auto_draft",
            "object_id": "138",
            "key": "ticket_no_138_agent_id_1_response_draft",
            "value": {
                "content": "<p>Work in progress reply</p>"
            },
            "created_at": null,
            "updated_at": null
        }
    }
```

This endpoint returns the current agent's saved reply draft for the ticket. `draft` is `null` when there is none.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/draft`

## Delete a reply draft

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<draft_id>/draft' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command discards the draft and returns the data in JSON.

```json
    {
        "message": "Discard draft successfully"
    }
```

This endpoint discards a saved reply draft. Note that the URL takes the **draft ID** (the `draftID` returned when saving), not the ticket ID. Agents can delete their own drafts; deleting another agent's draft needs ticket manage permission.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<draft_id>/draft`

## Approve a draft response

```shell

curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/approve_draft_response/<response_id>?content=<p>Approved reply content</p>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command publishes a draft response and returns the data in JSON.

```json
    {
        "message": "Draft response has been successfully approved.",
        "response": {
            "id": 160,
            "ticket_id": "138",
            "person_id": "3",
            "conversation_type": "response",
            "content": "<p>Approved reply content</p>",
            "source": "web",
            "created_at": "2021-12-06 10:00:00",
            "updated_at": "2021-12-06 10:05:00",
            "person": {
                "id": 3,
                "first_name": "John",
                "last_name": "Doe",
                "full_name": "John Doe",
                "photo": "https://www.gravatar.com/avatar/...?s=128"
            }
        }
    }
```

This endpoint approves a response saved as `draft_response` (by an agent with draft-only permission) and sends it to the customer. The agent needs the approve draft reply permission (`fst_approve_draft_reply`).

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/approve_draft_response/<response_id>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
content | text(HTML) | yes | The final content of the response. Send the original content to approve it unchanged.

## Ticket bulk actions

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-actions?bulk_action=assign_agent&ticket_ids[]=101&ticket_ids[]=102&agent_id=2' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command runs a bulk action on the selected tickets and returns the data in JSON.

```json
    {
        "message": "2 tickets have been assigned to John Doe."
    }
```

This endpoint runs one action on up to 200 tickets at a time. Tickets the agent can not access are skipped.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-actions`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
bulk_action | text | yes | The action to run, see the table below.
ticket_ids[] | array | yes | IDs of the tickets (max 200).
agent_id | int | for assign_agent | The agent to assign.
agent_group_id | int | for assign_agent_group | The agent group. Each ticket goes to the least loaded agent of the group.
tag_ids[] | array | for assign_tags / remove_tags | Ticket tag IDs.
priority | text | for change_priority | One of the admin ticket priorities (`normal`, `medium`, `critical`).
product_id | int | for change_product | An existing product ID.

***Possible bulk_action values:***

bulk_action | Extra params | Response message
----------- | ------------ | ----------------
close_tickets | | `%d tickets have been closed.`
delete_tickets | | `%d tickets have been deleted` (needs delete tickets permission)
reopen_tickets | | `%d tickets have been reopened.` (only closed tickets are reopened)
assign_agent | agent_id | `%d tickets have been assigned to {agent}.` (needs assign agents permission; tickets already assigned to the agent or in a mailbox the agent is restricted from are skipped)
assign_agent_group | agent_group_id | `%d tickets assigned via agent group. %d skipped.` (needs assign agents permission)
assign_tags | tag_ids[] | `Selected tags has been added to tickets`
remove_tags | tag_ids[] | `Selected tags have been removed from the tickets`
change_priority | priority | `Priority changed on %d tickets.`
change_product | product_id | `Product changed on %d tickets.`

## Bulk reply

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-reply?ticket_ids[]=101&ticket_ids[]=102&content=<p>We are looking into this.</p>&close_ticket=no' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command adds the same reply to the selected tickets and returns the data in JSON.

```json
    {
        "message": "Response has been added to the selected tickets"
    }
```

This endpoint adds the same reply to several tickets. Closed tickets and tickets the agent can not access are skipped.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-reply`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket_ids[] | array | yes | IDs of the tickets.
content | text(HTML) | yes | The reply content.
close_ticket | text(yes/no) | no | Use `yes` to close the tickets after replying.
attachments[] | array | no | File hashes of attachments uploaded by the current agent (not yet linked to a ticket).

## Get ticket sidebar counts

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/sidebar-counts' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "counts": {
            "statuses": {
                "open": 24,
                "active": 15,
                "closed": 90,
                "new": 9,
                "all": 114
            },
            "open": {
                "mine": 8,
                "unassigned": 5,
                "waiting": 12
            },
            "priorities": {
                "normal": 18,
                "medium": 4,
                "critical": 2
            }
        }
    }
```

This endpoint returns the ticket counts shown in the ticket list sidebar, limited to the tickets the current agent can see. `open` and `priorities` count open tickets only.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/sidebar-counts`

## Get saved searches

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/label-search' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    [
        {
            "filter_type": "advanced",
            "label_search_name": "Critical WooCommerce tickets",
            "advanced_filters": "[[{\"source\":[\"tickets\",\"priority\"],\"operator\":\"=\",\"value\":\"critical\"}]]",
            "id": 1
        }
    ]
```

This endpoint returns the current user's saved ticket searches (labels). The response is a plain array.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/label-search`

## Create or update a saved search

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/label-search?query[filter_type]=advanced&query[label_search_name]=Critical tickets&query[advanced_filters]=[[{"source":["tickets","priority"],"operator":"=","value":"critical"}]]' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command saves the search and returns the data in JSON.

```json
    {
        "message": "Your search has been saved successfully!"
    }
```

This endpoint saves an advanced ticket search for the current user. Pass `query[id]` to update an existing saved search (message: `Your saved search has been updated successfully!`). Only `advanced` filter types are saved; other types return `Invalid filter type.`

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/label-search`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
query[filter_type] | text | yes | Must be `advanced`.
query[label_search_name] | text | yes | Name of the saved search.
query[advanced_filters] | text(JSON) | yes | The advanced filter groups, same format as the ticket list `advanced_filters`.
query[id] | int | no | ID of the saved search to update.

## Delete a saved search

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<label_search_id>/label-search' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command deletes the saved search and returns the data in JSON.

```json
    {
        "message": "Search deleted successfully."
    }
```

This endpoint deletes one of the current user's saved searches. If the ID does not exist the message is `Search ID not found.`

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<label_search_id>/label-search`

## Get ticket live activity

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/live_activity' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "live_activity": [
            {
                "id": 1,
                "email": "john@example.com",
                "first_name": "John",
                "last_name": "Doe",
                "full_name": "John Doe",
                "photo": "https://www.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=128"
            }
        ]
    }
```

This endpoint returns the agents currently viewing the ticket (active in the last 60 seconds). Calling it also marks the current agent as viewing the ticket, so the admin UI polls it while a ticket is open.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/live_activity`

## Remove ticket live activity

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/live_activity' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command removes the current agent from the ticket's live activity and returns the data in JSON.

```json
    {
        "result": true,
        "agent_id": 1
    }
```

This endpoint removes the current agent from the ticket's viewers list (used when the agent leaves the ticket). `result` is `false` when there was no activity to remove.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/live_activity`

## Ping

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/ping' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "ping": "pong"
    }
```

This endpoint is a simple health check. It is useful to verify that the API credentials have access to the ticket endpoints.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/ping`

## Search contact

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/search-contact?search=john@example.com' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "type": "search_result",
        "provider": "fluent_support",
        "data": [
            {
                "first_name": "John",
                "last_name": "Doe",
                "email": "john@example.com",
                "id": 72,
                "user_id": null
            }
        ],
        "is_email": true,
        "search": "john@example.com"
    }
```

This endpoint finds a contact to create a ticket for. It searches Fluent Support customers first, then FluentCRM contacts (when FluentCRM is active), then WordPress users. `provider` is `fluent_support`, `fluent_crm`, `wp_users` or `none` (with `type` `none` and empty `data`). Up to 10 results are returned. Searching by partial name/email needs the sensitive data permission and at least 3 characters; otherwise a full email address is required. The search is rate limited per user (HTTP 429 when exceeded).

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/search-contact`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
search | text | yes | Full email address, or (with sensitive data permission) at least 3 characters of a name or email.

## Get widgets

```shell

curl --location -g --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/widgets?filter=ticket_sidebar&data[ticket_id]=138' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "widgets": []
    }
```

This endpoint returns the dynamic UI widgets registered by add-ons through the `fluent_support/widgets/{filter}` filter. The content of `widgets` depends entirely on the registered callbacks.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/widgets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
filter | text | yes | The widget area name (letters, numbers, `_` and `-`), for example `ticket_sidebar`. A `fluent_support_` prefix is removed.
data | array | no | Context passed to the filter. For `ticket_sidebar`, `data[ticket_id]` is required.

## Get FluentBooking event types

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-booking/event-types' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "status": {
            "active": true,
            "configured": true,
            "admin_url": "https://yourdomain.com/wp-admin/admin.php?page=fluent-booking#/",
            "message": ""
        },
        "event_types": [
            {
                "id": 3,
                "title": "30 Minute Support Call",
                "duration": 30,
                "calendar_title": "Support Team"
            }
        ]
    }
```

This endpoint returns the active, public FluentBooking event types that can be offered to customers. All FluentBooking endpoints need ticket manage permission and the FluentBooking plugin.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-booking/event-types`

## Get FluentBooking availability

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/fluent-booking/availability?event_type_id=3&range=next_3_days&timezone=UTC' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "availability": {
            "timezone": "UTC",
            "max_lookup_date": "2022-01-31 23:59:59",
            "days": [
                {
                    "date": "2021-12-06",
                    "label": "Monday, Dec 6",
                    "slots": [
                        {
                            "id": "2021-12-06_100000",
                            "start": "2021-12-06 10:00:00",
                            "time_label": "10:00 am"
                        }
                    ]
                }
            ]
        }
    }
```

This endpoint returns the available booking slots of an event type, grouped by day, so an agent can offer specific times in a reply.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/fluent-booking/availability`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
event_type_id | int | yes | FluentBooking event type ID.
range | text | no | `next_3_days` (default), `this_week`, `next_week`, `next_14_days` or `specific_dates`.
timezone | text | no | Timezone to show the slots in. Defaults to the calendar owner's timezone.
duration | int | no | Meeting duration in minutes, for events with multiple durations.
selected_dates[] | array | no | Dates (Y-m-d, max 7) when `range` is `specific_dates`.
calendar_month | text | no | Month (Y-m) when `range` is `specific_dates` and no dates are selected.

## Create FluentBooking link

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/fluent-booking/booking-link?event_type_id=3&message=Please pick a time&selected_slots[0][start]=2021-12-06 10:00:00&timezone=UTC' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "html": "<p>Please pick a time</p>...<p><a href=\"https://yourdomain.com/...\" target=\"_blank\" rel=\"noopener\">See all available times</a></p>",
        "plain_text": "Please pick a time\n..."
    }
```

This endpoint builds reply content (HTML and plain text) containing a tokenised booking link for the ticket's customer and the selected time slots. It does not send a reply; insert the returned content into a response.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/fluent-booking/booking-link`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
event_type_id | int | yes | FluentBooking event type ID.
message | text | no | Intro message shown above the slots.
selected_slots | array | no | Up to 10 slots, each with a `start` (Y-m-d H:i:s). Slots that are no longer available are dropped.
timezone | text | no | Timezone used to display the slots.

## Get FluentBooking meetings

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/fluent-booking/meetings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "meetings": {
            "upcoming": [
                {
                    "id": 12,
                    "event_title": "30 Minute Support Call",
                    "calendar_title": "Support Team",
                    "status": "scheduled",
                    "status_label": "Scheduled",
                    "start_time": "2021-12-06 10:00:00",
                    "local_start_time": "2021-12-06 10:00:00",
                    "timezone": "UTC",
                    "date_text": "10:00 am - 10:30 am, Monday, December 6, 2021",
                    "admin_url": "https://yourdomain.com/wp-admin/admin.php?page=fluent-booking#/scheduled-events?booking_id=12",
                    "join_url": "",
                    "internal_note": ""
                }
            ],
            "past": [],
            "message": ""
        }
    }
```

This endpoint returns up to 10 FluentBooking meetings of the ticket's customer, split into upcoming and past. When booking links were sent from the ticket, only bookings of those events made after the first link are returned.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/fluent-booking/meetings`

## Get Fluent Boards boards

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-boards/boards' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "boards": [
            {
                "id": 1,
                "title": "Support Escalations",
                "tasks": []
            }
        ]
    }
```

This endpoint returns the Fluent Boards boards. It returns an error (HTTP 400) when Fluent Boards is not installed or active.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-boards/boards`

## Get Fluent Boards stages

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-boards/stages/<board_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "stages": [
            {
                "id": 4,
                "title": "Open"
            },
            {
                "id": 5,
                "title": "In Progress"
            }
        ]
    }
```

This endpoint returns the stages of a Fluent Boards board.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-boards/stages/<board_id>`

## Create Fluent Boards task from ticket

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-boards/stages?source_id=<ticket_id>&board_id=1&stage_id=4&title=Investigate checkout bug&source=fluent-support' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command creates a task and returns the data in JSON.

```json
    {
        "message": "Task successfully added to Fluent Boards",
        "task": {
            "id": 31,
            "board_id": 1,
            "stage_id": 4,
            "title": "Investigate checkout bug",
            "source": "fluent-support",
            "source_id": 138
        }
    }
```

This endpoint creates a Fluent Boards task linked to a ticket. An internal note is added to the ticket and a comment to the task. Despite the URL, it creates a task, not a stage.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/fluent-boards/stages`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
source_id | int | yes | The ticket ID.
board_id | int | yes | Fluent Boards board ID.
stage_id | int | yes | Stage ID of the board.
title | text | yes | Task title.
description | text(HTML) | no | Task description.
source | text | no | Task source label.
crm_contact_id | int | no | FluentCRM contact to link with the task.
started_at | text | no | Start date (Y-m-d H:i:s).
due_at | text | no | Due date (Y-m-d H:i:s).

## Sync FluentCRM tags

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/sync-fluentcrm-tags?contact_id=15&tags[]=2&tags[]=5' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "tags": [
            {
                "id": 2,
                "title": "Customer",
                "slug": "customer"
            },
            {
                "id": 5,
                "title": "VIP",
                "slug": "vip"
            }
        ],
        "message": "FluentCRM contact tags has been updated"
    }
```

This endpoint sets a FluentCRM contact's tags to exactly the given list: missing tags are added, tags not in the list are removed. Requires FluentCRM and FluentCRM permission to manage contacts.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/sync-fluentcrm-tags`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
contact_id | int | yes | FluentCRM contact ID.
tags[] | array | no | FluentCRM tag IDs. Send an empty list to remove all tags.

## Sync FluentCRM lists

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/sync-fluentcrm-lists?contact_id=15&lists[]=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command returns JSON structured like this:

```json
    {
        "lists": [
            {
                "id": 1,
                "title": "Newsletter",
                "slug": "newsletter"
            }
        ],
        "message": "FluentCRM contact lists has been updated"
    }
```

This endpoint sets a FluentCRM contact's lists to exactly the given list: missing lists are added, other lists are removed. Requires FluentCRM and FluentCRM permission to manage contacts.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/sync-fluentcrm-lists`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
contact_id | int | yes | FluentCRM contact ID.
lists[] | array | no | FluentCRM list IDs. Send an empty list to remove the contact from all lists.
