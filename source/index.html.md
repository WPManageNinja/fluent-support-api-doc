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

This endpoint retrieves a specific ticket.

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

This endpoint will delete a specific customer.

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/customers/<customer_id>`