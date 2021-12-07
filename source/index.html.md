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

Endpoint to retrieve a specific ticket by Ticket ID.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ----------- | -----
with[] | array | Get additional ticket meta properties
with_data[] | array | Get additional ticket data

***Possible with parameters:***

- other_tickets
- extra_widgets
- fluentcrm_profile (property of with_data)
## Create a new Ticket using agent endpoint

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
close_ticket | text | no | To close the ticket use yes otherwise the value will be no.

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

curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-actions?ticket_ids[]=<ticket_id(s)>&bulk_action=delete_tickets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```
> The above command delete ticket(s) and returns the data in JSON.

```json
    {
        "message": "1 tickets have been deleted"
    }
```

This endpoint will delete ticket(s).

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/bulk-actions`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket_ids[] | array | yes | If you are going to delete multiple ticket then put the values as comma separated.
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

`POST https://yourdomain.com/wp-json/fluent-support/v2/tickets/<ticket_id>/responses/<response_id>`

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
prop_name | text | yes | prop_value's are ```client_priority, priority, agent_id, product_id```
prop_value | text/int | yes | This one will take value base on the prop_name if the prop_name takes id then you have to use a integer number otherwise you will use string.

# Customers

## Get all customers

```shell

curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customers?per_page=10&page=1&search=&status=all' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command get all customers.

```json
    {
    "customers": {
        "total": 41,
        "per_page": 10,
        "current_page": 1,
        "last_page": 5,
        "next_page_url": "/wp-json/fluent-support/v2/customers?page=2",
        "prev_page_url": null,
        "from": 1,
        "to": 10,
        "data": [
            {
                "id": 84,
                "first_name": "Rafi",
                "last_name": "Ahmed",
                "email": "dev@akxpert.com",
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
                "hash": "b8c610dc5994bd0125d65c5fb240c52b",
                "user_id": null,
                "description": null,
                "remote_uid": null,
                "last_response_at": "2021-12-04 16:10:54",
                "created_at": "2021-12-04 16:10:54",
                "updated_at": "2021-12-04 16:10:54",
                "total_tickets": 1,
                "total_responses": 0,
                "full_name": "Rafi Ahmed",
                "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128"
            },
            {
                "id": 82,
                "first_name": "Oscar",
                "last_name": "Shepherd",
                "email": "oscar@mailinator.com",
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
                "hash": "6b2517d17d5c83ef2b33ec3406e3c620",
                "user_id": null,
                "description": null,
                "remote_uid": null,
                "last_response_at": "2021-12-04 16:03:45",
                "created_at": "2021-12-04 16:03:45",
                "updated_at": "2021-12-04 16:03:45",
                "total_tickets": 1,
                "total_responses": 0,
                "full_name": "Oscar Shepherd",
                "photo": "https://www.gravatar.com/avatar/724cf89e60f8774db08be6389ae030b5?s=128"
            },
        ]
    }
}
```

Endpoint to get the list of available customers

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customers`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Define how many customer you want to collect in every page, default it's 10.
page | int | no | Define the page number, default 1.
search | text/int | no | Search customer by name, email or id.
status | text | no | filter customer by status active, inactive or all, default all.

## Get a specific customer

```shell

curl --location -g --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/customers/83?with[]=widgets&with[]=tickets&with[]=fluentcrm_profile' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command get a specific customer.

```json
   {
    "customer": {
        "id": 83,
        "first_name": "Rafi",
        "last_name": "Ahmed",
        "email": "dev@akxpert.com",
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
        "hash": "0345174902fb72145ebc9eeea1e93a99",
        "user_id": null,
        "description": null,
        "remote_uid": null,
        "last_response_at": "2021-12-04 16:06:33",
        "created_at": "2021-12-04 16:06:32",
        "updated_at": "2021-12-04 16:06:33",
        "full_name": "Rafi Ahmed",
        "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128"
    },
    "widgets": null,
    "tickets": [
        {
            "id": 143,
            "title": "Can I use Fluent Support with Fluent Forms",
            "status": "new",
            "customer_id": "83",
            "created_at": "2021-12-04 16:06:33"
        }
    ],
    "fluentcrm_profile": false
}
```

Endpoint to get a specific customer by customer ID

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
with[] | array | false | It takes widgets, tickets & fluentcrm_profile as value and return the data with the customer profile.

## Update a customer

```shell

curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/customers/83?email=dev@akxpert.com&first_name=Rafi&address_line_1=353 South Milton Court' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command update a customer.


```json
{
    "message": "Customer has been updated",
    "customer": {
        "id": 83,
        "first_name": "Rafi",
        "last_name": "Ahmed",
        "email": "dev@akxpert.com",
        "title": "",
        "avatar": null,
        "person_type": "customer",
        "status": "active",
        "ip_address": "",
        "last_ip_address": "",
        "address_line_1": "353 South Milton Court",
        "address_line_2": "33 Nobel Drive",
        "city": "168 Fabien Boulevard",
        "zip": "82363",
        "state": "435 Milton Avenue",
        "country": "",
        "note": "",
        "hash": "0345174902fb72145ebc9eeea1e93a99",
        "user_id": "0",
        "description": null,
        "remote_uid": "0",
        "last_response_at": "2021-12-04 16:06:33",
        "created_at": "2021-12-04 16:06:32",
        "updated_at": "2021-12-06 13:27:23",
        "full_name": "Rafi Ahmed",
        "photo": "https://www.gravatar.com/avatar/8dceee8bf8e7e7bf01cb9843120e7c0b?s=128"
    }
}
```

This endpoint will update a specific customer by cutomer ID

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
email | email | yes | Put customer email
first_name | text | yes | Put customer first name
last_name | text | no | Put customer last name
title | text | no | Customer title example: owner example co.
avatar | url | no | To change customer profile image put the image url here
status | text | no | There are two values available for this field active & inactive
address_line_1 | text | no | Customer address field one
address_line_2 | text | no | Customer address field two
city | text | no | Customer city field
zip | text/int | no | Customer zip code field
state | text | no | Customer state field
country | text | no | Customer country field
note | text | no | Add note for this customer

## Delete a customer

```shell

curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \

```

> The above command delete a customer.

```json

{
    "message": "Customer Deleted Successfully"
}

```

End point to delete a customber by Customer ID

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`

# Reports

## Get overall reports

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns overall reports.

```json
{
    "overall_reports": {
        "new_tickets": {
            "title": "New Tickets",
            "count": 109
        },
        "active_tickets": {
            "title": "Active Tickets",
            "count": 11
        },
        "closed_tickets": {
            "title": "Closed Tickets",
            "count": 11
        },
        "responses": {
            "title": "Responses",
            "count": 68
        }
    }
}
```

This endpoint will return ticket overall report.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports`

## Get Ticket Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/tickets-growth' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns ticket growth.

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

This endpoint will return ticket growth.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/tickets-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31

## Get Ticket Resolve Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/tickets-resolve-growth' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns closed tickets.

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

This endpoint will return closed tickets.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/tickets-resolve-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31

## Get Ticket Response Growth

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/response-growth' \
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

This endpoint will return the stats of total replies by agents.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/response-growth`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
date_range[] | date (YYYY-MM-DD) | no | To filter reports by date range use this param like date_range[]=2021-12-01&date_range[]=2021-12-31

## Get Agents Summary

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/reports/agents-summary' \
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
                "opens": "11",
                "closed": 3,
                "waiting_tickets": 2
            },
            "active_stat": {
                "average_waiting": "1 days",
                "max_waiting": "3 days",
                "waiting_tickets": "2"
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
                "opens": "50",
                "closed": 4,
                "waiting_tickets": 5
            },
            "active_stat": {
                "average_waiting": "4 days",
                "max_waiting": "2 weeks",
                "waiting_tickets": "5"
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
                "opens": "1",
                "closed": 8,
                "waiting_tickets": 6
            },
            "active_stat": {
                "average_waiting": "3 days",
                "max_waiting": "3 days",
                "waiting_tickets": "6"
            },
            "full_name": "John Murphy",
            "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
        }
    ]
}
```

This endpoint will returns total summary of agents.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/reports/agents-summary`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | no | Filter agent summary by date
to| date (YYYY-MM-DD) | no | Filter agent summary by date

# Get Personal Reports

## Get Overall Stats

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/my-reports' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns user's personal overall stats.

```json
{
    "overall_reports": {
        "replies_count": {
            "title": "Total Replies",
            "count": 559
        },
        "interactions_count": {
            "title": "Total Interactions",
            "count": 392
        },
        "total_closed": {
            "title": "Total Closed",
            "count": 55
        }
    }
}
```

This endpoint will return user's personal overall stats

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/my-reports`

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

This endpoint returns total ticket closed by user.

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

This endpoint returns total replies done by user.

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
[
    {
        "id": 26,
        "first_name": "Michael ",
        "last_name": "Smith",
        "stats": {
            "interactions": 18,
            "responses": 30,
            "opens": "50",
            "closed": 6,
            "waiting_tickets": 42
        },
        "active_stat": {
            "average_waiting": "4 days",
            "max_waiting": "2 weeks",
            "waiting_tickets": "42"
        },
        "full_name": "Michael  Smith",
        "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
    }
]
```

This endpoint returns total summary of the current agent.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/my-reports/my-summary`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
from | date (YYYY-MM-DD) | no | Filter agent summary by date
to| date (YYYY-MM-DD) | no | Filter agent summary by date

# Saved Replies

## Get Saved Replies

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns total replies done by user.

```json
{
    "total": 4,
    "per_page": 10,
    "current_page": 1,
    "last_page": 1,
    "next_page_url": null,
    "prev_page_url": null,
    "from": 1,
    "to": 4,
    "data": [
        {
            "id": 5,
            "created_by": "1",
            "mailbox_id": null,
            "product_id": "0",
            "title": "Closing Ticket ",
            "content": "<p>It has been my pleasure helping you.<br />Closing the tickets here. You can re-open it if you need any assistance.</p>\n<p>Thanks</p>",
            "created_at": "2021-11-15 18:36:07",
            "updated_at": "2021-11-15 18:36:07",
            "person": null,
            "product": null
        },
        {
            "id": 3,
            "created_by": "17",
            "mailbox_id": null,
            "product_id": "4",
            "title": "Fluent Forms Activation",
            "content": "<p>Hi this is me with your solution</p>\n<p>&nbsp;</p>",
            "created_at": "2021-11-15 11:05:33",
            "updated_at": "2021-11-15 18:36:57",
            "person": null,
            "product": {
                "id": 4,
                "source_uid": null,
                "mailbox_id": null,
                "title": "Fluent CRM",
                "description": "",
                "settings": null,
                "source": "local",
                "created_by": null,
                "created_at": "2021-11-11 09:42:52",
                "updated_at": "2021-11-11 09:42:52"
            }
        },
        {
            "id": 2,
            "created_by": "1",
            "mailbox_id": null,
            "product_id": "0",
            "title": "Email Change",
            "content": "<p>Hello Carl,</p>\n<p>Sure, would you mind sharing a little information? such as information about the organization and the website url etc?</p>\n<p>Also, you need to open a support ticket from the organization's official email address.</p>\n<p>We provide 50% discount to our non-profit clients.</p>\n<p>Thanks</p>",
            "created_at": "2021-11-12 12:46:48",
            "updated_at": "2021-11-12 12:46:48",
            "person": null,
            "product": null
        },
        {
            "id": 1,
            "created_by": "1",
            "mailbox_id": null,
            "product_id": "0",
            "title": "Installation Instruction",
            "content": "<p>This is how you can install</p>\n<p>Thanks</p>",
            "created_at": "2021-11-11 16:35:54",
            "updated_at": "2021-11-12 18:47:45",
            "person": null,
            "product": null
        }
    ]
}
```

This endpoint returns all saved replies templates.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/saved-replies`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
page | int | no | Current page, default 1
per_page | int | no | Render template per page, default 10
search | text | no | Search by any value

## Get a Specific Saved Reply

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> This endpoint returns a specific saved reply.

```json
{
    "id": 1,
    "created_by": "1",
    "mailbox_id": null,
    "product_id": "0",
    "title": "Installation Instruction",
    "content": "<p>This is how you can install</p>\n<p>Thanks</p>",
    "created_at": "2021-11-11 16:35:54",
    "updated_at": "2021-11-12 18:47:45",
    "person": null,
    "product": null
}
```

This endpoint returns a specific saved reply.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>`

## Add New Saved Reply

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command creates a new saved reply.

```json

{
    "reply": {
        "title": "Ticket Closing",
        "content": "<p>I am closing this ticket here. Let us know if you have any other issues.</p>",
        "product_id": "",
        "created_by": 1,
        "updated_at": "2021-12-06 18:16:51",
        "created_at": "2021-12-06 18:16:51",
        "id": 2
    },
    "message": "Reply Template has been created"
}

```

This endpoint creates a new saved reply.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/saved-replies`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | text | yes | Set a title for the saved reply template
content | text | yes | Set content of this reply
product_id | int | no | Set this reply for a specific product, but you cam use it anywhere in reply

## Update a Saved Reply Template

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will update a specific saved reply.

```json
{
    "message": "Reply Template has been updated",
    "reply": {
        "id": 1,
        "created_by": "1",
        "mailbox_id": null,
        "product_id": "2",
        "title": "Update License",
        "content": "<p>Please contact our billing department regarding this query.</p>",
        "created_at": "2021-12-01 15:55:28",
        "updated_at": "2021-12-06 18:23:35"
    }
}
```

This endpoint will update a specific saved reply.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
title | text | yes | Update the title for the saved reply template
content | text | yes | Update the content of this reply
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

This endpoint will delete a specific ticket.

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/saved-replies/<reply_id>`

# Activities

## Get Activities

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/activity-logger' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns activities

```json
{
    "activities": {
        "total": 189,
        "per_page": 10,
        "current_page": 1,
        "last_page": 19,
        "next_page_url": "/wp-json/fluent-support/v2/activity-logger?page=2",
        "prev_page_url": null,
        "from": 1,
        "to": 10,
        "data": [
            {
                "id": 507,
                "person_id": "85",
                "person_type": "customer",
                "event_type": "fluent_support/ticket_created",
                "object_id": "193",
                "object_type": "ticket",
                "description": "<a class=\"fs_link_trans fs_pr\" href=\"#view_customer\">Giulio Nieder</a> created a <a class=\"fs_link_trans fs_tk\" href=\"#view_ticket\">Ticket: Hello, I'm considering FluentSupport. I tried the free version, and t looks l... (#168)</a> via email",
                "created_at": "2021-12-06 09:31:02",
                "updated_at": "2021-12-06 09:31:02",
                "person": {
                    "first_name": "Giulio",
                    "person_type": "customer",
                    "last_name": "Nieder",
                    "id": 85,
                    "avatar": null,
                    "full_name": "Giulio Nieder",
                    "photo": "https://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=128"
                }
            }
        ]
    },
    "settings": {
        "delete_days": 14,
        "disable_logs": "no"
    }
}
```

This endpoint returns all activities

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/activity-logger`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Activities to show per page, default 10
page | int | no | Activity page number, default 1

## Get Activity Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns activity settings.

```json
{
    "activity_settings": {
        "delete_days": 1,
        "disable_logs": "no"
    }
}
```

This endpoint returns activity settings.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings`

## Update Activity Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command updates activity settings.

```json
{
    "message": "Activity settings has been updated"
}
```

This endpoint updates activity settings.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/activity-logger/settings`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
activity_settings[delete_days] | int | no | Delete activities automatically & you can define after how many days you want to delete this
activity_settings[disable_logs] | text | no | This takes yes/no value, use yes to disable log

# Mailbox

## Get Mailboxes

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command returns all mailboxes

```json
[
    {
        "id": 1,
        "name": "Fluent Support",
        "slug": "fluent-support",
        "box_type": "web",
        "email": "fluentsupport@mail.com",
        "mapped_email": "",
        "email_footer": "<p>Hi There,</p>\n<p>Here is your data.</p>\n<p>Customer First name: {{customer.first_name}}</p>\n<p>Customer Last name: {{customer.last_name}}</p>\n<p>Customer Email: {{customer.email}}</p>\n<p>Ticket ID: {{ticket.id}}</p>\n<p>Ticket Public URL: {{ticket.public_url}}</p>\n<p>Ticket Title: {{ticket.title}}</p>",
        "settings": {
            "admin_email_address": "fluentsupport@mail.com"
        },
        "avatar": "",
        "created_by": "1",
        "is_default": "yes",
        "created_at": "2021-11-18 15:08:42",
        "updated_at": "2021-11-25 18:45:27",
        "tickets_count": 113
    },
    {
        "id": 3,
        "name": "MailHog",
        "slug": "mailhog",
        "box_type": "web",
        "email": "mailhog@mail.com",
        "mapped_email": "",
        "email_footer": null,
        "settings": {
            "admin_email_address": "admin_mailhog@mail.com"
        },
        "avatar": null,
        "created_by": null,
        "is_default": "no",
        "created_at": "2021-11-20 18:49:54",
        "updated_at": "2021-11-20 18:49:54",
        "tickets_count": 137
    },
    {
        "id": 4,
        "name": "Fluent Support Help Desk",
        "slug": "fluent-support-help_desk",
        "box_type": "email",
        "email": "help.fluentsupport@mail.com",
        "mapped_email": "",
        "email_footer": null,
        "settings": {
            "admin_email_address": "admin.fluentsupport@mail.com"
        },
        "avatar": null,
        "created_by": null,
        "is_default": "no",
        "created_at": "2021-11-20 18:59:10",
        "updated_at": "2021-11-20 18:59:10",
        "tickets_count": 0
    }
]
```

This endpoint returns all mailboxes.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes`

## Get a Specific Mailbox

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command returns a specific mailbox

```json
{
    "mailbox": {
        "id": 1,
        "name": "Fluent Support",
        "slug": "fluent-support",
        "box_type": "web",
        "email": "fluentsupport@mail.com",
        "mapped_email": "",
        "email_footer": "<p>Hi There,</p>\n<p>Here is your data.</p>\n<p>Customer First name: {{customer.first_name}}</p>\n<p>Customer Last name: {{customer.last_name}}</p>\n<p>Customer Email: {{customer.email}}</p>\n<p>Ticket ID: {{ticket.id}}</p>\n<p>Ticket Public URL: {{ticket.public_url}}</p>\n<p>Ticket Title: {{ticket.title}}</p>",
        "settings": {
            "admin_email_address": "fluentsupport@mail.com"
        },
        "avatar": "",
        "created_by": "1",
        "is_default": "yes",
        "created_at": "2021-11-18 15:08:42",
        "updated_at": "2021-11-25 18:45:27",
        "tickets_count": 113
    },
}
```

This endpoint returns a specific mailbox.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>`

## Update a Mailbox

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command update a specific mailbox

```json
{
    "message": "Mailbox has been saved",
    "mailbox": {
        "id": 1,
        "name": "Fluent Support Email Box",
        "slug": "fluent-email-box",
        "box_type": "web",
        "email": "support@fluentsupport.com",
        "mapped_email": "",
        "email_footer": "<p>There You From Footer {{customer.first_name}}</p>\n<p>Ticket ID: {{ticket.id}}<br />Ticket Public URL: {{ticket.public_url}}<br />Ticket Title: {{ticket.title}}</p>",
        "settings": {
            "admin_email_address": "support@fluentsupport.com"
        },
        "avatar": "",
        "created_by": "1",
        "is_default": "yes",
        "created_at": "2021-11-10 10:43:36",
        "updated_at": "2021-12-06 14:32:43"
    }
}
```

This endpoint will update a specific mailbox.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
business[name] | text | Updates mailbox name
business[email] | email | Updates mailbox email
business[mapped_email] | email | Updates mailbox mapped email
business[settings][admin_email_address] | email | Updates mailbox admin email
business[box_type] | text | Updates mailbox type (web/email)
business[email_footer] | text | Updates mailbox email footer
business[is_default] | text | Define if the mailbox is default or not (yes/no)

## Get Mailbox Email Configs

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_configs' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns mailbox email configs in JSON structure

```json
    {
    "email_configs": [
        {
            "key": "ticket_created_email_to_customer",
            "title": "Ticket Created (To Customer)",
            "email_subject": "Re: {{ticket.title}} #{{ticket.id}}",
            "email_body": "<p>Hi <strong><em>{{customer.full_name}}</em>,</strong></p><p>Your request (<a href=\"{{ticket.public_url}}\">#{{ticket.id}}</a>) has been received, and is being reviewed by our support staff.</p><p>To add additional comments, follow the link below:</p><h4><a href=\"{{ticket.public_url}}\">View Ticket</a></h4><p>&nbsp;</p><p>or follow this link: {{ticket.public_url}}</p><hr /><p>{{business.name}}</p>",
            "status": "yes",
            "can_edit_subject": "yes",
            "description": "This email will be sent when a customer submit a support ticket"
        },
        {
            "key": "ticket_replied_by_agent_email_to_customer",
            "title": "Replied by Agent (To Customer)",
            "email_subject": "Re: {{ticket.title}} #{{ticket.id}}",
            "email_body": "<p>Hi <strong><em>{{customer.full_name}}</em>,</strong></p><p>An agent just replied to your ticket \"<strong>{{ticket.title}}</strong>\" (<a href=\"{{ticket.public_url}}\">#{{ticket.id}}</a>). To view his reply or add additional comments, click the button below:</p><h4><a href=\"{{ticket.public_url}}\">View Ticket</a></h4><p>or follow this link: {{ticket.public_url}}</p><hr /><p>Regards,<br />{{business.name}}</p>",
            "status": "yes",
            "can_edit_subject": "yes"
        },
        {
            "key": "ticket_closed_by_agent_email_to_customer",
            "title": "Ticket Closed by Agent (To Customer)",
            "email_subject": "Re: {{ticket.title}} #{{ticket.id}}",
            "email_body": "<p>Hi <strong><em>{{customer.full_name}},</strong></p><p>Your ticket - {{ticket.title}}</p><p>We hope that the ticket was resolved to your satisfaction. If you feel that the ticket should not be closed or if the ticket has not been resolved, please reopen the ticket (<a href=\"{{ticket.public_url}}\">#{{ticket.id}}</a>)</p><p>Regards,<br />{{business.name}}</p>",
            "status": "yes",
            "can_edit_subject": "yes",
            "description": "This email will be sent when an agent close a ticket"
        },
        {
            "key": "ticket_created_email_to_admin",
            "title": "Ticket Created (To Admin)",
            "email_subject": "New Ticket: {{ticket.title}} #{{ticket.id}}",
            "email_body": "<p>A new ticket (<a href=\"{{ticket.admin_url}}\">{{ticket.title}}</a>) as been submitted by {{customer.full_name}}</p><h4>Ticket Body</h4><p>{{ticket.content}}</p><p><b><a href=\"{{ticket.admin_url}}\">View Ticket</a></b></p>",
            "status": "yes",
            "can_edit_subject": "yes"
        },
        {
            "key": "ticket_replied_by_customer_email_to_admin",
            "title": "Replied by Customer (To Agent/Admin)",
            "email_subject": "New Response: {{ticket.title}} #{{ticket.id}}",
            "email_body": "<p>A new response has been added to \"<a href=\"{{ticket.admin_url}}\">{{ticket.title}}</a>\"  by {{customer.full_name}}</p><h4>Response Body</h4><p>{{response.content}}</p><p><b><a href=\"{{ticket.admin_url}}\">View Ticket</a></b></p>",
            "status": "yes",
            "can_edit_subject": "yes"
        }
    ],
    "email_keys": [
        "ticket_created_email_to_customer",
        "ticket_replied_by_agent_email_to_customer",
        "ticket_closed_by_agent_email_to_customer",
        "ticket_created_email_to_admin",
        "ticket_replied_by_customer_email_to_admin"
    ]
}
```

This endpoint returns mailbox email configs

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_configs`

## Get Mailbox Email Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_settings?email_type=<email_key>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the settings of a mailbox email.

```json
{
    "email_settings": {
        "key": "ticket_created_email_to_customer",
        "title": "Ticket Created (To Customer)",
        "email_subject": "Re: {{ticket.title}} #{{ticket.id}}",
        "email_body": "<p>Hi <strong><em>{{customer.full_name}}</em>,</strong></p><p>Your request (<a href=\"{{ticket.public_url}}\">#{{ticket.id}}</a>) has been received, and is being reviewed by our support staff.</p><p>To add additional comments, follow the link below:</p><h4><a href=\"{{ticket.public_url}}\">View Ticket</a></h4><p>&nbsp;</p><p>or follow this link: {{ticket.public_url}}</p><hr /><p>{{business.name}}</p>",
        "status": "yes",
        "can_edit_subject": "yes",
        "description": "This email will be sent when a customer submit a support ticket"
    }
}
```

This endpoint returns mailbox email settings

***All available email keys are:***

- ticket_replied_by_agent_email_to_customer
- ticket_created_email_to_customer
- ticket_closed_by_agent_email_to_customer
- ticket_created_email_to_admin
- ticket_replied_by_customer_email_to_admin

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_settings?email_type=<email_key>`

### Update Email Settings

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command updates mailbox email setting.

```json
{
    "message": "Settings has been updated"
}
```

This endpoint update mailbox email settings

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_settings`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
email_type - required | text | Define the email type
email_settings[key] - required | text | Define email settings key
email_settings[title] | text | Update email settings title
email_settings[email_subject] | text | Update email subject
email_settings[email_body] | text | Update email body
email_settings[status] | text | Enable or disable the email `yes` to enable and use `no` to disable
email_settings[can_edit_subject] | text | Enable or disable the subject editing `yes` to enable and use `no` to disable
email_settings[description] | text | Email description

## Delete Mailbox
```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete a mailbox

```json
{
    "message": "Selected Business has been deleted"
}
```

This endpoint will delete mailbox.


### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
fallback_id - required | int | To delete a mailbox you must need to transfer all the tickets associated with this mailbox to another mailbox, so put the targeted mailbox id here

# Workflow

## Get Workflows
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above endpoint returns available workflows

```json
{
    "workflows": {
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
                "created_by": "1",
                "priority": "10",
                "title": "Add Internal Note",
                "trigger_key": "",
                "trigger_type": "manual",
                "settings": "",
                "status": "published",
                "last_ran_at": "0000-00-00 00:00:00",
                "created_at": "2021-12-07 15:14:29",
                "updated_at": "2021-12-07 15:15:48"
            },
            {
                "id": 4,
                "created_by": "1",
                "priority": "10",
                "title": "Assign Agent",
                "trigger_key": "fluent_support/ticket_created",
                "trigger_type": "automatic",
                "settings": {
                    "conditions": [
                        [
                            {
                                "data_key": "message.title",
                                "data_operator": "contains",
                                "data_value": "Fluent Support"
                            }
                        ]
                    ]
                },
                "status": "published",
                "last_ran_at": "0000-00-00 00:00:00",
                "created_at": "2021-12-07 15:08:37",
                "updated_at": "2021-12-07 15:09:25",
                "trigger_human_name": "On Ticket Creation"
            }
        ]
    }
}
```
This endpoint returns available workflows.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows`

### URL Parameters
Parameters | Type | Description
---------- | ---- | -----------
per_page | int | How many workflows you want to show in page
page | int | Current page number
search |  text | Search workflows by any specific value

## Get a Specific Workflow
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific workflow

```json
{
    "actions": [
        {
            "id": 15,
            "title": "Add Internal Note",
            "action_name": "fs_action_create_note",
            "workflow_id": "5",
            "settings": {
                "response_body": "<p>We need to test this.</p>"
            },
            "created_at": "2021-12-07 15:15:48",
            "updated_at": "2021-12-07 15:15:48"
        }
    ],
    "action_fields": {
        "fs_action_create_response": {
            "title": "Add Response",
            "settings_defaults": {
                "response_body": ""
            },
            "fields": {
                "response_body": {
                    "type": "wp-editor",
                    "label": "Response Body"
                }
            }
        },
        "fs_action_assign_agent": {
            "title": "Assign Agent",
            "settings_defaults": {
                "agent_id": "",
                "skip_if_exist": "yes"
            },
            "fields": {
                "agent_id": {
                    "type": "agent-selectors",
                    "label": "Select Agent",
                    "extra_options": [
                        {
                            "id": "unassigned",
                            "title": "Unassigned"
                        }
                    ]
                },
                "skip_if_exist": {
                    "type": "inline-checkbox",
                    "true_label": "yes",
                    "false_label": "no",
                    "checkbox_label": "Skip if ticket already have a agent assigned"
                }
            }
        },
        "fs_action_create_note": {
            "title": "Add Internal Note",
            "settings_defaults": {
                "response_body": ""
            },
            "fields": {
                "response_body": {
                    "type": "wp-editor",
                    "label": "Note Body"
                }
            }
        },
        "fs_action_close_ticket": {
            "title": "Close Ticket",
            "settings_defaults": {
                "agent_id": "",
                "fallback_agent": ""
            },
            "fields": []
        },
        "fs_action_add_tags": {
            "title": "Add Tag(s)",
            "settings_defaults": {
                "tag_ids": []
            },
            "fields": {
                "tag_ids": {
                    "is_multiple": true,
                    "label": "Select Tags",
                    "type": "tag-selectors",
                    "placeholder": "Select Tags"
                }
            }
        },
        "fs_action_remove_tags": {
            "title": "Remove Tag(s)",
            "settings_defaults": {
                "tag_ids": []
            },
            "fields": {
                "tag_ids": {
                    "is_multiple": true,
                    "label": "Select Tags",
                    "type": "tag-selectors",
                    "placeholder": "Select Tags"
                }
            }
        },
        "fs_delete_ticket": {
            "title": "Delete Ticket",
            "settings_defaults": {
                "ticket_delete_html": ""
            },
            "fields": {
                "ticket_delete_html": {
                    "type": "html-viewer",
                    "html": "<p><br />Ticket will be deleted permanently and no further action will run</p>"
                }
            }
        },
        "fs_block_customer": {
            "title": "Block Ticket Submitter (Customer)",
            "settings_defaults": {
                "customer_block_html": ""
            },
            "fields": {
                "customer_block_html": {
                    "type": "html-viewer",
                    "html": "<p><br />Customer will be blocked and can not create new ticket or access to previous tickets</p>"
                }
            }
        }
    },
    "trigger_fields": {
        "triggers": {
            "fluent_support/ticket_created": {
                "title": "On Ticket Creation",
                "description": "This workflow will be initiated on when a new ticket has been submitted by customer",
                "supported_conditions": [
                    "customer.first_name",
                    "customer.last_name",
                    "customer.email",
                    "message.title",
                    "message.content",
                    "message.attachments",
                    "ticket.client_priority",
                    "ticket.mailbox_id",
                    "message.added_time_range",
                    "message.added_date_range",
                    "ticket.product_id"
                ]
            },
            "fluent_support/response_added_by_customer": {
                "title": "On Customer Response",
                "description": "Workflow will be initiated when customer add a response to an existing ticket",
                "supported_conditions": [
                    "customer.first_name",
                    "customer.last_name",
                    "customer.email",
                    "message.content",
                    "message.attachments",
                    "ticket.agent_id",
                    "ticket.mailbox_id",
                    "message.added_time_range",
                    "message.added_date_range",
                    "ticket.product_id"
                ]
            }
        },
        "conditions": {
            "customer.first_name": {
                "title": "Customer First Name",
                "data_type": "string",
                "group": "Customer"
            },
            "customer.last_name": {
                "title": "Customer Last Name",
                "data_type": "string",
                "group": "Customer"
            },
            "customer.email": {
                "title": "Customer Email",
                "data_type": "string",
                "group": "Customer"
            },
            "message.title": {
                "title": "Ticket Title",
                "data_type": "string",
                "group": "Message"
            },
            "message.content": {
                "title": "Message Content",
                "data_type": "string",
                "group": "Message"
            },
            "message.attachments": {
                "title": "Attachments",
                "data_type": "yes_no",
                "default": "yes",
                "options": {
                    "yes": "Has an attachment",
                    "no": "Does not have an attachment"
                },
                "group": "Message"
            },
            "message.added_time_range": {
                "title": "Added Time Range",
                "data_type": "time_range",
                "group": "Message"
            },
            "message.added_date_range": {
                "title": "Added Date Range",
                "data_type": "date_range",
                "group": "Message"
            },
            "ticket.client_priority": {
                "title": "Ticket Priority (Client)",
                "data_type": "single_dropdown",
                "options": {
                    "normal": "Normal",
                    "medium": "Medium",
                    "critical": "Critical"
                },
                "group": "Ticket"
            },
            "ticket.product_id": {
                "title": "Selected Product",
                "data_type": "single_dropdown",
                "options": {
                    "1": "Fluent Support",
                    "2": "Fluent Forms"
                },
                "group": "Ticket"
            }
        }
    },
    "workflow": {
        "id": 5,
        "created_by": "1",
        "priority": "10",
        "title": "Add Internal Note",
        "trigger_key": "",
        "trigger_type": "manual",
        "settings": "",
        "status": "published",
        "last_ran_at": "0000-00-00 00:00:00",
        "created_at": "2021-12-07 15:14:29",
        "updated_at": "2021-12-07 15:15:48"
    }
}
```

This endpoint will return a specific workflow.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
with[] | text | This takes two param, `action_fields` & `trigger_fields`

## Update a Workflow

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command will update a specific workflow by id

```json
{
    "message": "Workflow has been updated",
    "workflow": {
        "id": 6,
        "created_by": "1",
        "priority": "10",
        "title": "Close Ticket",
        "trigger_key": "fluent_support/response_added_by_customer",
        "trigger_type": "automatic",
        "settings": {
            "conditions": [
                [
                    {
                        "data_key": "message.content",
                        "data_operator": "contains",
                        "data_value": "Thank You"
                    }
                ]
            ]
        },
        "status": "draft",
        "last_ran_at": "",
        "created_at": "2021-12-07 17:54:15",
        "updated_at": "2021-12-07 17:54:44"
    },
    "actions": [
        {
            "id": 17,
            "title": "Close Ticket",
            "action_name": "fs_action_close_ticket",
            "workflow_id": "6",
            "settings": {
                "agent_id": "ticket_agent_id",
                "fallback_agent": "1"
            },
            "created_at": "2021-12-07 17:54:44",
            "updated_at": "2021-12-07 17:54:44"
        }
    ]
}
```

This endpoint update a specific workflow by id.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
actions[0][action_name] - required | text | Define the specific action by action name
actions[0][title] | text | Update action title
actions[0][settings][setting_key_here] -required | text | Specify the setting key
workflow[priority] | int | Update workflow priority
workflow[title] | text | Update workflow title

## Create a New Workflow
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/workflows' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will create a new workflow

```json
{
    "message": "Workflow has been updated",
    "workflow": {
        "id": 7,
        "created_by": "1",
        "priority": "10",
        "title": "Close Ticket",
        "trigger_key": "fluent_support/response_added_by_customer",
        "trigger_type": "automatic",
        "settings": {
            "conditions": [
                [
                    {
                        "data_key": "message.content",
                        "data_operator": "contains",
                        "data_value": "Thanks You"
                    }
                ]
            ]
        },
        "status": "published",
        "last_ran_at": "",
        "created_at": "2021-12-07 18:11:08",
        "updated_at": "2021-12-07 18:11:53"
    },
    "actions": [
        {
            "id": 22,
            "title": "Close Ticket on Thank You",
            "action_name": "fs_action_close_ticket",
            "workflow_id": "7",
            "settings": {
                "agent_id": "ticket_agent_id",
                "fallback_agent": "1"
            },
            "created_at": "2021-12-07 18:11:53",
            "updated_at": "2021-12-07 18:11:53"
        }
    ]
}
```

This endpoint will create a new workflow.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/workflows`

### URL Parameters
Parameters | Type | Description
---------- | ---- | -----------
actions[0][title] | text | Provide action title
actions[0][action_name] | text | Specify the action by action key
workflow[priority] | int | Specify the workflow priority
workflow[title] | text | Specify the workflow title
workflow[trigger_key] | text | Specify the trigger key
workflow[trigger_type] | text | Specify the trigger type manual or automatic
workflow[settings][conditions][0][0][data_key] | text | Condition data key
workflow[settings][conditions][0][0][data_operator] | mixed | Define condition logic
workflow[settings][conditions][0][0][data_value] | mixed | Condition value
workflow[status] | text | Define workflow status

## Delete a Workflow

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/workflows/<workflow_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete a specific workflow by id

```json
{
    "message": "Selected workflow has been deleted"
}
```

This endpoint will delete a specific workflow by id
