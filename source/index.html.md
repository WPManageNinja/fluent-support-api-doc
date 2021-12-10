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

Welcome to the Fluent Support API document. This document will describe the REST API Endpoints of Fluent Support.

The example API documentation page was created with [Slate](https://github.com/slatedocs/slate). If you find any typo or would like contribute please send a pull request with improvements. Please note that, All endpoints are not added to this doc (Work in progress).

# Authentication

Fluent Support uses WordPress REST API. So you can use any authorization method that supports WordPress. The easiest way to connect is application password. To create a application password go to the **Users** and click on the user and create a new application password for this user so that this person can easily access the REST API endpoint by using WP Username & Application password in basic auth system.

Once you create your Application Password in WordPress, Add Authorization Header to every request.

In the basic auth username section use your WP Username and in the password section use the created Application Password.
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
                "email": "rafi@authlab.io",
                "first_name": "Rafi",
                "last_name": "Ahmed",
                "full_name": "Rafi Ahmed",
                "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128"
            }
        ],
        "custom_fields": [],
        "customer": {
            "id": 72,
            "first_name": "Aston",
            "last_name": "Ager",
            "email": "aston@mail.com",
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
            "photo": "https://www.gravatar.com/avatar/4e4113e6ee2838e07ee1745b6eff6231?s=128"
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
        "tags": [],
        "attachments": []
    },
    "responses": [],
    "agent_id": 1
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

This endpoint creates a new ticket. Using this endpoint you will be able to create a new Customer as well as a WP user. However, to create new customer there are some fields you have to use.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-crm/v2/tickets`

### URL Parameters to create ticket

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
ticket[create_customer] | text(yes/no) | no | Default value should be no or blank
ticket[create_wp_user] | text(yes/no) | no | Default value should be no or blank
ticket[customer_id] | int | yes | Specify the customer for whom you are creating this ticket.
ticket[mailbox_id] | int | yes | Select the mailbox.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.
ticket[client_priority] | text | no | set the client/customer ticket priority.


###  URL Parameters to Create customer during ticket creation:

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket[create_customer] | text(yes/no) | yes | Pass yes as value to create customer
newCustomer[first_name] | text | yes | Add your customer first name.
newCustomer[last_name] | text | yes | Add your customer last name.
newCustomer[email] | email | yes | Add your customer email.
ticket[create_wp_user] | text(yes/no) | no | Default value should be no or blank
ticket[mailbox_id] | int | yes | Select the mailbox.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.
ticket[client_priority] | text | no | set the client/customer ticket priority.

###  URL Parameters to Create customer and WP user during ticket creation:

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
ticket[create_customer] | text(yes/no) | yes | Pass yes as value to create customer
newCustomer[first_name] | text | yes | Add your customer first name.
newCustomer[last_name] | text | yes | Add your customer last name.
newCustomer[email] | email | yes | Add your customer email.
ticket[create_wp_user] | text(yes/no) | yes | Pass yes as value to create WP user
newCustomer[username] | text | yes | WP username
newCustomer[password] | password | yes | WP user's password
ticket[mailbox_id] | int | yes | Select the mailbox.
ticket[title] | text | yes | Set the ticket title.
ticket[content] | text | yes | Add ticket contents.
ticket[product_id] | int | no | specify the product.
ticket[client_priority] | text | no | set the client/customer ticket priority.
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


# Global Settings

## Get Global Settings

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/settings?settings_key=global_business_settings&with%5B%5D=fields' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return global business settings

```json
{
    "settings": {
        "portal_page_id": "547",
        "login_message": "<p>Please login or create an account to access the Customer Support Portal</p> [fluent_support_auth]",
        "disable_public_ticket": "no",
        "accepted_file_types": [
            "images",
            "csv",
            "documents",
            "zip",
            "json"
        ],
        "max_file_size": "2",
        "del_files_on_close": "no"
    },
    "fields": {
        "portal_page_id": {
            "type": "input-options",
            "label": "Portal Page",
            "show_id": true,
            "placeholder": "Select Portal Page",
            "options": [
                {
                    "id": "555",
                    "title": "Submit Ticket"
                },
                {
                    "id": "554",
                    "title": "Support Desk"
                },
                {
                    "id": "551",
                    "title": "FF Ticket"
                },
                {
                    "id": "547",
                    "title": "Support Portal"
                },
            ],
            "inline_help": "Please provide the page id where you want to show the tickets for your customers. Use shortcode <code>[fluent_support_portal]</code> in that page"
        },
        "login_message": {
            "type": "wp-editor",
            "label": "Message for non logged in users",
            "inline_help": "Please provide message for not logged in users. You can place login shortcode too Use shortcode <code>[fluent_support_login]</code> to show built-in login form. For the user registration use this shortcode <code>[fluent_support_signup]</code> and for both form please use <code>[fluent_support_auth]</code>"
        },
        "disable_public_ticket": {
            "type": "inline-checkbox",
            "true_label": "yes",
            "false-label": "no",
            "checkbox_label": "Disable Public Ticket interaction",
            "inline_help": "If you enable this then only logged in user can reply the tickets. Otherwise, url will be signed and intended user can reply without logging in"
        },
        "accepted_file_types": {
            "wrapper_class": "fs_half_field",
            "type": "checkbox-group",
            "label": "Accepted File Types",
            "options": {
                "images": "Photos",
                "csv": "CSV",
                "documents": "PDF/Docs",
                "zip": "Zip",
                "json": "JSON"
            }
        },
        "max_file_size": {
            "wrapper_class": "fs_half_field",
            "type": "input-text",
            "data_type": "number",
            "label": "Max File Size (in MegaByte)"
        },
        "del_files_on_close": {
            "type": "inline-checkbox",
            "true_label": "yes",
            "false-label": "no",
            "checkbox_label": "Delete all attachments on ticket close",
            "inline_help": "If you enable this then when a ticket get closed it will delete all the attachments associated with the particular ticket."
        }
    }
}
```

This endpoint will return global business settings

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/settings`

### URL Parameters

Parameters | Required | Type | Description
---------- | -------- | ---- | -----------
settings_key | yes | text | Use `global_business_settings` as `settings_key` value to get all settings
with[] | yes | text | Use `fields` to get all settings field


## Update Global Settings

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will update the settings

```json
{
    "message": "Settings has been updated"
}
```

This endpoint will update settings

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/settings`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
settings_key - required | text | Define the settings key
settings[portal_page_id] | int | Change the customer support page
settings[login_message] | HTML Supported | Change login page message
settings[disable_public_ticket] | text | Disable public ticket opening by using `yes`, to keep it enable use no
settings[accepted_file_types][] | text | Accepted file types, `images`, `csv`, `documents`, `zip`, `json` are available file types
settings[max_file_size] | int | Maximum allowed file size it will be defined as megabytes
settings[del_files_on_close] | text | To delete ticket attachments on ticket closing use `yes`, use `no` to keep the attachments

## Get Tags

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command will return all tags

```json

```

This endpoint will return all tags

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags`

### URL Parameters
Parameters | Type | Description
---------- | ---- | -----------
per_page |  int | Records per page | 15
page | int | Page Number for Pagination | 1
search | string | Search by available methods

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

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>`

## Create a New Tag
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will create a new tag

```json

```
### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
title | text | Add ticket tag title
description | text | Add ticket tag description


## Update a Tag

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
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

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
title | text | Update tag title
description | text | Update tag description


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

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/ticket-tags/<tag_id>`

## Get Ticket Form Config
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will returns ticket form config

```json
{
    "settings": {
        "enable_docs": "yes",
        "docs_post_types": [
            "post",
            "course",
        ],
        "post_limits": "5",
        "disable_rich_text": "no",
        "disabled_fields": [
            "product_services"
        ],
        "submitter_type": "allowed_user_roles",
        "allowed_user_roles": [],
        "field_labels": {
            "subject": "Subject",
            "ticket_details": "Ticket Details",
            "details_help": "Please provide details about your problem",
            "product_services": "Related Product/Service",
            "priority": "Priority",
            "btn_text": "Create Ticket",
            "submit_heading": "Submit a Support Ticket",
            "create_ticket_cta": "Create a New Ticket"
        },
        "enable_woo_menu": "yes"
    },
    "settings_fields": {
        "enable_docs": {
            "type": "inline-checkbox",
            "checkbox_label": "Enable knowledge base suggestion on ticket creation form",
            "true_label": "yes",
            "false_label": "no"
        },
        "docs_post_types": {
            "type": "checkbox-group",
            "label": "Knowledge Base post types",
            "options": {
                "post": "post",
                "course": "course",
            },
            "inline_help": "Select the post types that you want to show articles from",
            "wrapper_class": "fs_half_field",
            "dependency": {
                "depends_on": "enable_docs",
                "operator": "=",
                "value": "yes"
            }
        },
        "post_limits": {
            "type": "input-text",
            "data_type": "number",
            "label": "Suggested Articles Limit",
            "wrapper_class": "fs_half_field",
            "dependency": {
                "depends_on": "enable_docs",
                "operator": "=",
                "value": "yes"
            }
        },
        "disabled_fields": {
            "type": "checkbox-group",
            "label": "Disabled Default Fields",
            "wrapper_class": "fs_half_field",
            "options": {
                "file_upload": "File Upload",
                "priority": "Priority",
                "product_services": "Product & Services"
            },
            "inline_help": "Checked fields will not be available on create ticket form"
        },
        "disable_rich_text": {
            "type": "inline-checkbox",
            "checkbox_label": "Disable Rich Text Editor for Frontend",
            "true_label": "yes",
            "wrapper_class": "fs_half_field",
            "false_label": "no"
        },
        "submitter_type": {
            "type": "input-radio",
            "label": "Who can access customer portal?",
            "options": [
                {
                    "id": "logged_in_users",
                    "label": "Any logged in users"
                },
                {
                    "id": "allowed_user_roles",
                    "label": "Only selected user roles"
                }
            ]
        },
        "allowed_user_roles": {
            "type": "checkbox-group",
            "label": "Select Users Roles for Customer Portal",
            "options": {
                "author": "Author",
                "contributor": "Contributor",
                "customer": "Customer",
                "editor": "Editor",
                "subscriber": "Subscriber",
            },
            "dependency": {
                "depends_on": "submitter_type",
                "operator": "=",
                "value": "allowed_user_roles"
            }
        },
        "field_labels": {
            "label": "Form Labels Customization",
            "source_label": "Field",
            "new_label": "Input Label",
            "type": "object-tabular-input",
            "options": {
                "subject": "Subject heading",
                "ticket_details": "Form content heading",
                "details_help": "Content help message",
                "product_services": "Product/Service heading",
                "priority": "Priority heading",
                "btn_text": "Create ticket button text",
                "submit_heading": "Create ticket page heading",
                "create_ticket_cta": "Ticket Create Call to Action"
            }
        },
        "enable_woo_menu": {
            "type": "inline-checkbox",
            "checkbox_label": "Add support link to WooCommerce account navigation",
            "true_label": "yes",
            "false_label": "no"
        }
    }
}
```

This endpoint will returns ticket form config

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
with[] - required | text | This will render settings fields & use `fields` to do this

## Update Ticket Form Config

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will update ticket form config

```json
{
    "message": "Settings has been updated"
}
```
This endpoint will update ticket form config

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/pro/form-settings`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
settings[enable_docs] | text | Enable or disable knowledge base docs suggestion option
settings[docs_post_types][] | text | Select post type from where you want to suggest the knowledge base
settings[post_limits] | int | Define how many post you want to show in knowledge base suggestion
settings[disable_rich_text] | text | Enable and disable rich text editing
settings[disabled_fields][] | text | Disable default field
settings[submitter_type] | text | Use `allowed_user_roles` to enable this
settings[allowed_user_roles][] | text | After enabling `submitter_type` you can give access to the user role
settings[field_labels][field_key_here] | text | Change field label
settings[enable_woo_menu] | text | Enable or disable support navigation in woocommerce customer account page

## Get Products

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/products' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return all products

```json
{
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
            "id": 2,
            "source_uid": null,
            "mailbox_id": null,
            "title": "Fluent Forms",
            "description": "",
            "settings": null,
            "source": "local",
            "created_by": null,
            "created_at": "2021-11-29 17:56:13",
            "updated_at": "2021-11-29 17:56:13"
        },
        {
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
        }
    ]
}
```

This endpoint will return all products

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/products`

### URL Parameters
Parameter | Type | Description
---------- | ---- | -----------
per_page |  int | Records per page | 15
page | int | Page Number for Pagination | 1
search | string | Search by available methods

## Get a Specific Product

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific product by it's id

```json
{
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
    }
}
```

This endpoint will return a specific product by it's id

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`

## Create a New Product
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/products' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command will create a new product

```json
{
    "message": "Product has been successfully created",
    "product": {
        "title": "Fluent CRM",
        "description": "",
        "updated_at": "2021-12-09 16:11:13",
        "created_at": "2021-12-09 16:11:13",
        "id": 3
    }
}
```

This endpoint will create a new product

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/products`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
title | text | Add product title
description | text | Add product description



## Update a Product

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will return a specific product by it's id

```json
{
    "message": "Product has been updated",
    "product": {
        "id": 1,
        "source_uid": "0",
        "mailbox_id": null,
        "title": "Fluent Support",
        "description": "This is for Fluent Support",
        "settings": "",
        "source": "local",
        "created_by": "0",
        "created_at": "2021-11-29 17:56:07",
        "updated_at": "2021-12-09 14:44:35"
    }
}
```

This endpoint will return a specific product by it's id

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`


### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
title | text | Update product title
description | text | Update product description

## Delete a Product

```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will delete a specific product by it's id

```json
{
    "message": "Product has been deleted"
}
```

This endpoint will delete a specific product by it's id

### HTTP Request

`DELETE https://yourdomain.com/wp-json/fluent-support/v2/products/<product_id>`

## Get All Support Staff
```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/agents' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will returns all agents

```json
{
    "agents": {
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
                "permissions": [
                    "fst_view_dashboard",
                    "fst_manage_own_tickets",
                    "fst_manage_unassigned_tickets",
                    "fst_manage_other_tickets",
                    "fst_delete_tickets",
                    "fst_manage_settings",
                    "fst_sensitive_data",
                    "fst_manage_workflows",
                    "fst_run_workflows",
                    "fst_view_all_reports",
                    "fst_manage_saved_replies",
                    "fst_view_activity_logs",
                    "administrator"
                ],
                "user_profile": "https://fs.app/wp-admin/user-edit.php?user_id=1",
                "replies_count": 60,
                "interactions_count": 19,
                "telegram_chat_id": "683330659",
                "slack_user_id": "",
                "full_name": "Rafi Ahmed",
                "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128"
            }
        ]
    },
    "permissions": [
        {
            "title": "Tickets Permissions",
            "permissions": {
                "fst_view_dashboard": "View Dashboard",
                "fst_manage_own_tickets": "Manage Own Tickets",
                "fst_manage_unassigned_tickets": "Manage Unassigned Tickets",
                "fst_manage_other_tickets": "Manage Others Tickets",
                "fst_delete_tickets": "Delete Tickets"
            }
        },
        {
            "title": "Workflow Permissions",
            "permissions": {
                "fst_manage_workflows": "Manage Workflows",
                "fst_run_workflows": "Run workflows",
                "fst_manage_saved_replies": "Manage Saved Replies"
            }
        },
        {
            "title": "Settings",
            "permissions": {
                "fst_manage_settings": "Manage Overall Settings",
                "fst_sensitive_data": "Access Private Data (Customers, Agents)"
            }
        },
        {
            "title": "Reporting",
            "permissions": {
                "fst_view_all_reports": "View All Reports",
                "fst_view_activity_logs": "View Activity Logs"
            }
        }
    ]
}
```

This endpoint will returns all agents

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/agents`

### URL Parameters
Parameter | Type | Description
---------- | ---- | -----------
per_page |  int | Records per page | 15
page | int | Page Number for Pagination | 1
search | string | Search by available methods

## Create a New Support Agent
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/agents' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will create a new agent

```json

```

This endpoint will create a new agent

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/agents`

### URL Parameters
Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
email | yes | email | Add agent email address
first_name | yes | text | Add agent first name
last_name | no | text | Add agent last name
title | no | text | Add agent title
permissions[] | yes | text | Add agent permissions by permission keys
telegram_chat_id | no | mixed | Add agent telegram chat id for integration
slack_user_id | no | mixed | Add agent slack user id for integration

## Update an Agent
```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will update an agent profile by id

```json
{
    "message": "Support Staff has been updated",
    "agent": {
        "id": 1,
        "first_name": "Rafi",
        "last_name": "Ahmed",
        "email": "rafi@authlab.io",
        "title": "Agent",
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
        "updated_at": "2021-12-09 16:49:02",
        "telegram_chat_id": "683330659",
        "slack_user_id": "",
        "full_name": "Rafi Ahmed",
        "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128"
    }
}
```

This endpoint will update an agent profile by id

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>`

### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
first_name | text | Update agent first name
last_name | text | Update agent last name
title | text | Update agent title
permissions[] | text | Update agent permissions by permission keys
telegram_chat_id | mixed | Update agent telegram chat id for integration
slack_user_id | mixed | Update agent slack user id for integration
avatar | url | Update agent profile image

## Delete an Agent
```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command will delete an agent by id

 ```json
{
    "message": "Selected support staff has been deleted"
}
 ```
This endpoint will delete an agent by id

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>`


### URL Parameters
Parameter | Type | Description
--------- | ---- | -----------
fallback_agent_id - required | int | You need to define fallback agent id here
