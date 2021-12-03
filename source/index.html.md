---
title: Fluent Support REST API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php

toc_footers:
  - <a href='https://fluentsupport.com/docs'>User Documentation</a>
  - <a href='https://wordpress.org/plugins/fluent-support/'>Download Fluent Support</a>
  - <a href='https://fluentsupport.com'>Fluent Support Official Website</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to Fluent Support API doc. This doc will describe the REST API Endpoints of Fluent Support.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). If you find any typo or would like contribute please send a pull request with improvements. Please note that, All endpoints are not added to this doc (Work in progress).

# Authentication

Fluent Support uses WordPress REST API. So you can use any authorization method that supports WordPress.

Once you create your Application Password in WordPress, Add Authorization Header to every request.

> Example API Call for tickets

```shell
# With shell, you can just pass the correct header with each request
curl "https://yourdomain.com/wp-json/fluent-support/v2/" \
  -H "Authorization: BASIC API_USERNAME:API_PASSWORD"
```

**API Base URL:** `https://yourdomain.com/wp-json/fluent-support/v2`

> Make sure to replace `API_USERNAME` & `API_PASSWORD` with your UserName & API Password.

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
                    "email": "hillary@icloud.com",
                    "id": 2,
                    "full_name": "Hillary Sander",
                    "photo": "https://www.gravatar.com/avatar/1e6fd8e56879c84999cd481255530592?s=128"
                },
                "agent": {
                    "first_name": "Rafi",
                    "last_name": "Ahmed",
                    "id": 1,
                    "full_name": "Rafi Ahmed",
                    "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
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
| search | string | Search by available methods | |
| order_by | string | Ticket Sort By(use sorting key) | |
| order_type | string | ASC/DESC | |
| statuses | array | Status slugs to filter contacts | |
| filters | array | Get some specific ticket by filtering the ticket | |

***Possible Filter Values:***

- status_type (open/active/new/closed/all)
- product_id (int)
- agent_id (int)
- priority (Agent TIcket Priority)
- client_priority (Customer TIcket Priority)
- waiting_for_reply (yes/no)
- mailbox_id (int)
- ticket_tags (int:id)

<aside class="success">
Remember — Use authentication Headers
</aside>

## Get a Specific Ticket


```shell
curl "https://yourdomain.com/wp-json/fluent-support/v2/ticket/<ID>" \
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
                    "email": "hillary@icloud.com",
                    "id": 2,
                    "full_name": "Hillary Sander",
                    "photo": "https://www.gravatar.com/avatar/1e6fd8e56879c84999cd481255530592?s=128"
                },
                "agent": {
                    "first_name": "Rafi",
                    "last_name": "Ahmed",
                    "id": 1,
                    "full_name": "Rafi Ahmed",
                    "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
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

This endpoint retrieves a specific ticket.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ----------- | -----
with[] | array | Get Additional Ticket Meta Properties

***Possible with parameters:***

- other_tickets
- extra_widgets

## Create a new Ticket using agent endpoint

```shell

curl --location -g --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets?ticket[customer_id]=72&ticket[mailbox_id]=1&ticket[title]=Does Fluent Support comes with REST API?&ticket[content]=Is REST API available in Fluent Support?&ticket[product_id]=1&ticket[client_priority]=normal&ticket[create_customer]=no&ticket[create_wp_user]=no' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command creates a new contact in Fluent CRM and returns the data in JSON.

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
            "email": "rafilisan2@gmail.com",
            "mapped_email": "",
            "email_footer": "<p>Hi There,</p>\n<p>Here is your data.</p>\n<p>Customer First name: {{customer.first_name}}</p>\n<p>Customer Last name: {{customer.last_name}}</p>\n<p>Customer Email: {{customer.email}}</p>\n<p>Ticket ID: {{ticket.id}}</p>\n<p>Ticket Public URL: {{ticket.public_url}}</p>\n<p>Ticket Title: {{ticket.title}}</p>",
            "settings": {
                "admin_email_address": "rafilisan2@gmail.com"
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

This endpoint creates a new ticket. Using this endpoint you can also create a new Customer and at the same time you can create a WP user. However, to create new customer there are some fields you have to use.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-crm/v2/tickets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket[customer_id] | int | yes | Specify the customer for whom you are creating this ticket.
ticket[mailbox_id] | int | yes | Select the mailbox.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.
ticket[client_priority] | text | no | set the client/customer ticket priority.

###If you also want to create a customer on ticket creation you can use below parameters:

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket[create_customer] | text(yes/no) | no | Use yes to create a new customer when a ticket is submitting and if your are doing this then you don't need to use ```ticket[customer_id]```
ticket[create_wp_user] | text(yes/no) | no | If you use yes then it will create a WP user profile for this customer along with customer creation.
newCustomer[first_name] | text | no | Add your customer first name.
newCustomer[last_name] | text | no | Add your customer last name.
newCustomer[email] | email | yes | Add your customer email.
newCustomer[username] | text | yes | If you are going to create a new WP user also then this field is required.
newCustomer[password] | password | yes | Create user password.


## Create a new Ticket using customer endpoint

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/customer-portal/tickets?title=About Features&content=Does Fluent Support has custom fields' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command creates a new contact in Fluent CRM and returns the data in JSON.

```json
    {
    "message": "Ticket has been created successfully",
    "ticket": {
        "title": "About Features",
        "content": "Does Fluent Support has custom fields?",
        "customer_id": 27,
        "product_source": "local",
        "mailbox_id": 1,
        "priority": "",
        "client_priority": "",
        "source": "web",
        "slug": "about-features",
        "hash": "c1e880131",
        "last_customer_response": "2021-12-03 15:31:00",
        "content_hash": "1e4ef873fb4010ca6d1e5da68b5a45a8",
        "created_at": "2021-12-03 15:31:00",
        "updated_at": "2021-12-03 15:31:00",
        "waiting_since": "2021-12-03 15:31:00",
        "id": 138,
        "custom_fields": [],
        "mailbox": {
            "id": 1,
            "name": "Fluent Support",
            "slug": "fluent-support",
            "box_type": "web",
            "email": "rafilisan2@gmail.com",
            "mapped_email": "",
            "email_footer": "<p>Hi There,</p>\n<p>Here is your data.</p>\n<p>Customer First name: {{customer.first_name}}</p>\n<p>Customer Last name: {{customer.last_name}}</p>\n<p>Customer Email: {{customer.email}}</p>\n<p>Ticket ID: {{ticket.id}}</p>\n<p>Ticket Public URL: {{ticket.public_url}}</p>\n<p>Ticket Title: {{ticket.title}}</p>",
            "settings": {
                "admin_email_address": "rafilisan2@gmail.com"
            },
            "avatar": "",
            "created_by": "1",
            "is_default": "yes",
            "created_at": "2021-11-18 15:08:42",
            "updated_at": "2021-11-25 18:45:27"
        },
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
custom_data | array | no | Use custom field slug to send value to the field custom field.

## Reply to a ticket as agent

```shell

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses?content=Yes, fluent support does have custom fields.&conversation_type=note' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command creates a new contact in Fluent CRM and returns the data in JSON.

```json
    {
    "message": "Response has been added",
    "response": {
        "person_id": 1,
        "ticket_id": 138,
        "conversation_type": "note",
        "content": "<p>Yes, fluent support does have custom fields.</p>\n",
        "source": "web",
        "content_hash": "c7d7f144acde427c2ff8c7471cd3b715",
        "updated_at": "2021-12-03 17:01:21",
        "created_at": "2021-12-03 17:01:21",
        "id": 114,
        "person": {
            "id": 1,
            "first_name": "Rafi",
            "last_name": "Ahmed",
            "email": "rafi@authlab.io",
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
            "full_name": "Rafi Ahmed",
            "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128"
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
conversation_type | text | yes | There are two type available response & note, use response to add reply to a ticket and for adding a internal note to this ticket then use note.
close_ticket | text | no | Use custom field slug to send value to the field custom field.