# Agents

Listing agents and uploading or resetting an agent avatar require the `fst_sensitive_data` permission. Creating, updating and deleting agents require a site administrator (`manage_options`), because agent records carry plugin permissions; other users get a 403 error: "Only administrators can add, edit, or delete support staff."

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
                "first_name": "John",
                "last_name": "Doe",
                "full_name": "John Doe",
                "email": "john@example.com",
                "title": "Developer",
                "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
                "avatar": null,
                "user_id": "1",
                "status": "active",
                "person_type": "agent",
                "created_at": "2021-11-18 15:07:45",
                "updated_at": "2021-12-02 20:14:35",
                "telegram_chat_id": "123456789",
                "slack_user_id": "",
                "whatsapp_number": "",
                "restrictions": {
                    "restrictedBusinessBoxes": [],
                    "businessBoxRestrictions": false
                },
                "agent_signature": "",
                "agent_signature_enabled": "no",
                "groups": [
                    {
                        "id": 3,
                        "title": "Support Team"
                    }
                ],
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
                "replies_count": 60,
                "interactions_count": 19,
                "user_profile": "https://yourdomain.com/wp-admin/user-edit.php?user_id=1"
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
            "title": "Settings",
            "permissions": {
                "fst_manage_settings": "Manage Overall Settings",
                "fst_sensitive_data": "Access Private Data (Customers, Agents)"
            }
        }
    ],
    "businessBoxes": [
        {
            "id": 1,
            "name": "Support"
        }
    ]
}
```

This endpoint will returns all agents, along with the available permission groups and the mailboxes that can be used for agent restrictions.

### HTTP Request
`GET https://yourdomain.com/wp-json/fluent-support/v2/agents`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
per_page | int | no | Records per page
page | int | no | Page Number for Pagination, default 1
search | string | no | Search agents by name or email

## Create a New Support Agent
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/agents' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "jane@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "permissions": ["fst_view_dashboard", "fst_manage_own_tickets"]
}'
```

> The above command will create a new agent

```json
{
    "message": "Support Staff has been added",
    "agent": {
        "id": 5,
        "first_name": "Jane",
        "last_name": "Doe",
        "full_name": "Jane Doe",
        "email": "jane@example.com",
        "title": "",
        "photo": "https://www.gravatar.com/avatar/9e26471d35a78862c17e467d87cddedf?s=128",
        "avatar": null,
        "user_id": 12,
        "status": "active",
        "person_type": "agent",
        "created_at": "2021-12-09 16:49:02",
        "updated_at": "2021-12-09 16:49:02",
        "telegram_chat_id": "",
        "slack_user_id": "",
        "whatsapp_number": "",
        "restrictions": {
            "restrictedBusinessBoxes": [],
            "businessBoxRestrictions": false
        },
        "agent_signature": "",
        "agent_signature_enabled": "no"
    }
}
```

This endpoint will create a new agent. A WordPress user with the given email must already exist, and no other agent/person may use that email. The new agent is added to the default agent group, if one is set. Requires a site administrator (`manage_options`).

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/agents`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
email | email | yes | Email address of an existing WordPress user
first_name | text | yes | Agent first name
last_name | text | no | Agent last name
title | text | no | Agent title
permissions[] | array | no | Agent permission keys (e.g. `fst_manage_own_tickets`)
telegram_chat_id | text | no | Telegram chat id for integration
slack_user_id | text | no | Slack user id for integration
whatsapp_number | text | no | WhatsApp number for integration
restrictions | object | no | Mailbox restrictions: `businessBoxRestrictions` (boolean) and `restrictedBusinessBoxes` (array of mailbox IDs)
agent_signature | text/html | no | Agent reply signature
agent_signature_enabled | text | no | `yes` or `no`

## Update an Agent
```shell
curl --location --request PUT 'https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--header 'Content-Type: application/json' \
--data-raw '{
    "first_name": "John",
    "last_name": "Doe",
    "title": "Agent"
}'
```

> The above command will update an agent profile by id

```json
{
    "message": "Support Staff has been updated",
    "agent": {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe",
        "full_name": "John Doe",
        "email": "john@example.com",
        "title": "Agent",
        "photo": "https://www.gravatar.com/avatar/3f6e19a6d1fcf98c73e031882796091f?s=128",
        "avatar": null,
        "user_id": "1",
        "status": "active",
        "person_type": "agent",
        "created_at": "2021-11-18 15:07:45",
        "updated_at": "2021-12-09 16:49:02",
        "telegram_chat_id": "123456789",
        "slack_user_id": "",
        "whatsapp_number": "",
        "restrictions": {
            "restrictedBusinessBoxes": [],
            "businessBoxRestrictions": false
        },
        "agent_signature": "",
        "agent_signature_enabled": "no"
    }
}
```

This endpoint will update an agent profile by id. The email always stays the linked WordPress user's email. Permissions and settings are only changed when they are sent. Requires a site administrator (`manage_options`).

### HTTP Request
`PUT https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
first_name | text | yes | Agent first name
last_name | text | no | Agent last name
title | text | no | Agent title
permissions[] | array | no | Agent permission keys; replaces the current permissions
telegram_chat_id | text | no | Telegram chat id for integration
slack_user_id | text | no | Slack user id for integration
whatsapp_number | text | no | WhatsApp number for integration
restrictions | object | no | Mailbox restrictions: `businessBoxRestrictions` (boolean) and `restrictedBusinessBoxes` (array of mailbox IDs)
agent_signature | text/html | no | Agent reply signature
agent_signature_enabled | text | no | `yes` or `no`

## Delete an Agent
```shell
curl --location --request DELETE 'https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>?fallback_agent_id=1' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```
> The above command will delete an agent by id

```json
{
    "message": "Support Staff has been deleted"
}
```

This endpoint will delete an agent by id. The agent's tickets, replies, attachments and products are moved to the fallback agent. Requires a site administrator (`manage_options`).

### HTTP Request
`DELETE https://yourdomain.com/wp-json/fluent-support/v2/agents/<agent_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
fallback_agent_id | int | yes | The agent who takes over this agent's tickets (must be a different agent)

## Upload Agent Avatar
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/agents/avatar/<agent_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
--form 'file=@"/path/to/avatar.png"'
```

> The above command will upload a new profile image for an agent

```json
{
    "message": "Profile picture has been updated successfully",
    "image": "https://yourdomain.com/wp-content/uploads/fluent-support/agent_avatars/avatar.png",
    "agent": {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe",
        "email": "john@example.com",
        "avatar": "https://yourdomain.com/wp-content/uploads/fluent-support/agent_avatars/avatar.png",
        "person_type": "agent",
        "status": "active",
        "full_name": "John Doe",
        "photo": "https://yourdomain.com/wp-content/uploads/fluent-support/agent_avatars/avatar.png"
    }
}
```

This endpoint uploads a profile image for an agent as a multipart form request. Allowed file types are jpeg, jpe, jpg and png.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/agents/avatar/<agent_id>`

### URL Parameters
Parameter | Type | Required| Description
--------- | ---- | ------- | -----------
file | file | yes | The image file (multipart form field `file`)

## Reset Agent Avatar
```shell
curl --location --request POST 'https://yourdomain.com/wp-json/fluent-support/v2/agents/reset_avatar/<agent_id>' \
--header 'Authorization: BASIC API_USERNAME:API_PASSWORD' \
```

> The above command will reset an agent's avatar

```json
{
    "message": "Support Staff avatar reset to gravatar default"
}
```

This endpoint removes the uploaded profile image of an agent, so the Gravatar image is used again.

### HTTP Request
`POST https://yourdomain.com/wp-json/fluent-support/v2/agents/reset_avatar/<agent_id>`
