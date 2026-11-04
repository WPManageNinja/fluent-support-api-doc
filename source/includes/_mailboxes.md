# Mailbox

## Get Mailboxes

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command returns all mailboxes

```json
{
  "mailboxes": [
    {
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
        "updated_at": "2021-11-25 18:45:27",
        "tickets_count": 113
    },
    {
        "id": 3,
        "name": "MailHog",
        "slug": "mailhog",
        "box_type": "web",
        "email": "mailhog@example.com",
        "mapped_email": "",
        "email_footer": null,
        "settings": {
            "admin_email_address": "admin_mailhog@example.com"
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
        "email": "help.support@example.com",
        "mapped_email": "",
        "email_footer": null,
        "settings": {
            "admin_email_address": "admin.support@example.com"
        },
        "avatar": null,
        "created_by": null,
        "is_default": "no",
        "created_at": "2021-11-20 18:59:10",
        "updated_at": "2021-11-20 18:59:10",
        "tickets_count": 0
    }
  ]
}
```

This endpoint returns all mailboxes, each with its `tickets_count`.

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
    }
}
```

This endpoint returns a specific mailbox.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>`

## Create a Mailbox

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "business": {
        "name": "Sales",
        "email": "sales@example.com",
        "box_type": "web"
    }
}'
```

> The above command creates a new mailbox

```json
{
    "message": "Mailbox has been created successfully",
    "mailbox": {
        "name": "Sales",
        "email": "sales@example.com",
        "box_type": "web",
        "mapped_email": "",
        "email_footer": "",
        "is_default": "no",
        "settings": {
            "admin_email_address": "sales@example.com"
        },
        "slug": "sales",
        "created_by": 1,
        "updated_at": "2026-11-04 10:12:30",
        "created_at": "2026-11-04 10:12:30",
        "id": 5
    }
}
```

This endpoint creates a new mailbox. For a `web` mailbox the admin notification address is set to the mailbox email; for an `email` (piped) mailbox it is left empty until piping is set up. The first mailbox created is always the default.

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/mailboxes`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
business[name] | string | yes | Mailbox name
business[email] | email | yes | Mailbox email
business[box_type] | string | no | Mailbox type: `web` or `email`. Send it: when omitted the type is saved as an empty string
business[mapped_email] | email | no | Mailbox mapped email
business[email_footer] | string | no | Mailbox email footer (HTML)
business[is_default] | string | no | `yes` to flag the mailbox as default

## Update a Mailbox

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "business": {
        "name": "Fluent Support Email Box",
        "email": "support@example.com",
        "box_type": "web",
        "mapped_email": "",
        "email_footer": "<p>There You From Footer {{customer.first_name}}</p>",
        "is_default": "yes",
        "settings": {
            "admin_email_address": "support@example.com"
        }
    }
}'
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
        "email": "support@example.com",
        "mapped_email": "",
        "email_footer": "<p>There You From Footer {{customer.first_name}}</p>\n<p>Ticket ID: {{ticket.id}}<br />Ticket Public URL: {{ticket.public_url}}<br />Ticket Title: {{ticket.title}}</p>",
        "settings": {
            "admin_email_address": "support@example.com"
        },
        "avatar": "",
        "created_by": "1",
        "is_default": "yes",
        "created_at": "2021-11-10 10:43:36",
        "updated_at": "2021-12-06 14:32:43"
    }
}
```

This endpoint will update a specific mailbox. The update replaces the whole record, so send every field: an omitted `box_type`, `mapped_email` or `email_footer` is saved as an empty string, and an omitted `is_default` is saved as `no` (which takes the default flag off the default mailbox). `settings` is only changed when it is sent.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
business[name] | string | yes | Mailbox name
business[email] | email | yes | Mailbox email
business[box_type] | string | no | Mailbox type: `web` or `email`. Saved as an empty string when omitted
business[mapped_email] | email | no | Mailbox mapped email. Required when `box_type` is `email`. Saved as an empty string when omitted
business[settings][admin_email_address] | email | no | Mailbox admin email
business[email_footer] | string | no | Mailbox email footer (HTML). Saved as an empty string when omitted
business[is_default] | string | no | `yes` to flag the mailbox as default, anything else (or omitted) is saved as `no`

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
        },
        {
            "key": "ticket_agent_on_change",
            "title": "Ticket Agent Change (To Agent)",
            "email_subject": "Ticket Agent Change: {{ticket.title}} #{{ticket.public_id}}",
            "email_body": "<p>Hi <strong><em>{{agent.full_name}}</em>,</strong></p><p>Ticket \"<a href=\"{{ticket.admin_url}}\">#{{ticket.public_id}}</a>\" assigned to you.</p>",
            "status": "yes",
            "can_edit_subject": "yes",
            "send_attachments": "no"
        },
        {
            "key": "ticket_created_by_agent_email_to_customer",
            "title": "Ticket Logged by Agent (To Customer)",
            "email_subject": "Re: {{ticket.title}} #{{ticket.public_id}}",
            "email_body": "<p>Hi <strong><em>{{customer.full_name}}</em>,</strong></p><p>{{agent.full_name}} created a ticket on behalf of you, you can check it <a href=\"{{ticket.public_url}}\">here</a></p>.",
            "status": "no",
            "can_edit_subject": "yes",
            "send_attachments": "no"
        },
        {
            "key": "ticket_created_by_agent_on_behalf_email_to_customer",
            "title": "Agent Outreach Ticket (To Customer)",
            "email_subject": "{{ticket.title}}",
            "email_body": "{{response.full_content}}{{agent.signature}}<p><a href=\"{{ticket.public_url}}\">Reply to this conversation</a></p>",
            "status": "yes",
            "can_edit_subject": "yes",
            "send_attachments": "no"
        }
    ],
    "email_keys": [
        "ticket_created_email_to_customer",
        "ticket_replied_by_agent_email_to_customer",
        "ticket_closed_by_agent_email_to_customer",
        "ticket_created_email_to_admin",
        "ticket_replied_by_customer_email_to_admin",
        "ticket_agent_on_change",
        "ticket_created_by_agent_email_to_customer",
        "ticket_created_by_agent_on_behalf_email_to_customer"
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
- ticket_agent_on_change
- ticket_created_by_agent_email_to_customer
- ticket_created_by_agent_on_behalf_email_to_customer

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_settings?email_type=<email_key>`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
email_type | string | yes | The email key (one of the keys listed above)

## Update Mailbox Email Settings

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/email_settings' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email_type": "ticket_created_email_to_customer",
    "email_settings": {
        "key": "ticket_created_email_to_customer",
        "title": "Ticket Created (To Customer)",
        "email_subject": "Re: {{ticket.title}} #{{ticket.id}}",
        "email_body": "<p>Hi {{customer.full_name}}, we received your request.</p>",
        "status": "yes",
        "can_edit_subject": "yes"
    }
}'
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

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
email_type | string | yes | The email key to update (one of the keys listed above)
email_settings[email_subject] | string | yes | Email subject
email_settings[email_body] | string | yes | Email body (HTML)
email_settings[key] | string | no | Email settings key
email_settings[title] | string | no | Email settings title
email_settings[status] | string | no | `yes` to enable the email, `no` to disable it
email_settings[can_edit_subject] | string | no | `yes` or `no`
email_settings[send_attachments] | string | no | `yes` or `no`

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

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
fallback_id | int | yes | To delete a mailbox you must need to transfer all the tickets associated with this mailbox to another mailbox, so put the targeted mailbox id here. It can not be the mailbox being deleted. If the deleted mailbox was the default, the fallback mailbox becomes the default

## Set Default Mailbox

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/set_default' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command sets a mailbox as the default one

```json
{
    "message": "Selected Business has been set as default"
}
```

This endpoint sets the given mailbox as the default mailbox. All other mailboxes are flagged as not default.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/set_default`

## Get Mailbox Tickets

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/tickets?filters[ticket_title]=refund&page=1&per_page=10' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the tickets of a mailbox

```json
{
    "tickets": {
        "current_page": 1,
        "data": [
            {
                "id": 118,
                "customer_id": 7,
                "agent_id": 2,
                "mailbox_id": 1,
                "product_id": 3,
                "title": "Refund request",
                "status": "active",
                "priority": "normal",
                "created_at": "2026-11-01 08:15:22",
                "updated_at": "2026-11-02 11:40:05",
                "customer": {
                    "id": 7,
                    "first_name": "John",
                    "last_name": "Doe",
                    "email": "john@example.com",
                    "avatar": null
                },
                "agent": {
                    "id": 2,
                    "first_name": "Jane",
                    "last_name": "Agent"
                },
                "product": {
                    "id": 3,
                    "title": "Fluent Support"
                },
                "tags": [],
                "preview_response": null
            }
        ],
        "per_page": 15,
        "total": 1,
        "last_page": 1
    }
}
```

This endpoint returns the paginated tickets of a mailbox (used by the "move tickets" screen), with the customer, agent, product, tags and the latest response of each ticket.

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/tickets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
filters[customer_id] | int | no | Only tickets of this customer
filters[product_id] | int | no | Only tickets of this product
filters[ticket_title] | string | no | Only tickets whose title contains this text
page | int | no | Page number
per_page | int | no | Tickets per page

## Move Mailbox Tickets

```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/move_tickets' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "new_box_id": 3,
    "move_type": "selected",
    "ticket_ids": [118, 119]
}'
```

> The above command moves tickets to another mailbox

```json
{
    "message": "All ticket moves to the selected Business"
}
```

This endpoint moves tickets from this mailbox to another mailbox. The request fails if the new mailbox is the same as the current one, if you are restricted from the new mailbox, or if an agent assigned to one of the tickets is restricted from the new mailbox.

### HTTP Request

`PUT https://yourdomain.com/wp-json/fluent-support/v2/mailboxes/<mailbox_id>/move_tickets`

### URL Parameters

Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
new_box_id | int | yes | The mailbox ID the tickets will be moved to
move_type | string | no | `All` to move every ticket of the mailbox. Any other value moves only `ticket_ids`
ticket_ids | array | yes, unless `move_type` is `All` | Array of ticket IDs to move

## Get Email Piping Status

```shell
curl --location --request GET 'https://yourdomain.com/wp-json/fluent-support/v2/email-box/<mailbox_id>/status' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command returns the email piping status of a mailbox

```json
{
    "email_pipe": {
        "status": "active",
        "mapped_email": "abc123@mail.fluentsupport.com"
    },
    "is_custom_supported": false,
    "webhook_url": "https://yourdomain.com/wp-json/fluent-support/v2/mail-piping/4/push/BOX_TOKEN",
    "error_message": ""
}
```

This endpoint returns the email piping status of an `email` type mailbox. If no mapped email has been issued yet, `email_pipe.status` is `not_issued` and `mapped_email`/`error_message` are not returned. The status is fetched from the Fluent Support piping server and requires an active license; otherwise `status` is `unknown` and `error_message` explains why. A `web` type mailbox returns an error.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`GET https://yourdomain.com/wp-json/fluent-support/v2/email-box/<mailbox_id>/status`

## Issue Mapped Email

```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/email-box/<mailbox_id>/issue-email' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command issues a mapped (forwarding) email address for the mailbox

```json
{
    "message": "Mailbox mapped email has been generated",
    "email_pipe": {
        "masked_email_id": "abc123@mail.fluentsupport.com",
        "mapped_email": "abc123@mail.fluentsupport.com"
    }
}
```

This endpoint requests a new mapped email address from the Fluent Support piping server and saves it as the mailbox's `mapped_email`. Emails forwarded to that address are pushed back to your site as tickets/replies. It only works for `email` type mailboxes that do not have a mapped email yet and requires an active Fluent Support Pro license. The `email_pipe` object is the piping server's response.

<aside class="notice">This endpoint requires Fluent Support Pro.</aside>

### HTTP Request

`POST https://yourdomain.com/wp-json/fluent-support/v2/email-box/<mailbox_id>/issue-email`
